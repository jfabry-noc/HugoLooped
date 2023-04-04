---
title: "kinit Fails Only When Using Keytab File"
date: 2022-09-07T18:26:27-04:00
tags:
  - kinit
  - keytab
  - Kerberos
draft: false
---

Lately I've been working through getting WinRM connectivity working between a Linux container and a bunch of Windows servers. I'm using the venerable [pywinrm library](https://pypi.org/project/pywinrm/). It works _great_, but there was a decent bit of setup for the underlying host to make it work that I had been unfamiliar with; you can't just create a client object, plug in some credentials, and go. A big part of this for my setup was configuring [krb5](https://pkgs.alpinelinux.org/package/edge/main/aarch64/krb5) to be able to speak to Active Directory appropriately.

My setup involves a container that runs an SSH server which another, external service actually SSHs into in order to execute various pieces of code. So my idea was to take the entrypoint script that configures the SSH server and have it also both:

1. Create a keytab file.
2. Use it to get a TGT.
3. Create a `cron` job to keep it refreshed.

Let's pretend the AD account I had been given to use was:

**Username@sub.domain.com**

In my manual testing, this worked fine after I was prompted for the password:

```shell
kinit Username@SUB.DOMAIN.COM
```

If you're completely new to this, note that it's actually critical that the domain (more appropriately called the "realm" in this case) is in all capital letters. If I run this manually by `exec`ing my way into a container, I get a TGT just like I'd expect. I can view it via:

```shell
klist -e
```

Unfortunately, things didn't go smoothly when I tried to use a keytab file. I created one in my entrypoint shell script via a function that runs:

```shell
{
    echo "addent -password -p Username@SUB.DOMAIN.COM -k 1 -e aes256-cts-hmac-sha1-96"
    sleep 1
    echo <password>
    sleep 1
    echo "wkt /file.keytab"
} | ktutil &> /dev/null
```

The keytab file is created successfully, but as soon as I try to leverage it with...

```shell
kinit Username@SUB.DOMAIN.COM -kt /file.keytab
```

...I receive a Kerberos preauthentication error. After much confusion and searching around online, I finally found an [article](https://community.cloudera.com/t5/Community-Articles/quot-kinit-Preauthentication-failed-while-getting-initial/ta-p/346998) that got me on the right track.

The article discusses the fact that an assumption is being made under the hood that the salt being used to encrypt the contents of the keytab file is the realm concatenated together with the user's samAccountName (aka "shortname"). So for my sample account, the salt value would be:

**SUB.DOMAIN.COMUsername**

The problem highlighted by the article is that when you authenticate via the UserPrincipalName format (e.g.: username@domain.com) rather than the shortname format (e.g.: domain\username), another assumption is made that the prefix of the UPN is the same as the shortname. This is very commonly not the case; in a previous life where I actually was the AD sysadmin, I had shortnames of first initial and last name while the UPNs were actually firstname dot lastname. So for example, my UPN was:

**looped.network@domain.com**

While my samAccountName was:

**lnetwork**

If this type of mismatch happens, you can use `-s` when running `addent` to specify the salt. After checking AD, I verified in my current case that the username was the same for both properties... but that in both places it was completely _lowercase_. I can't say why it was given to me with the first character capitalized, but after re-trying with **username@SUB.DOMAIN.COM**, everything was successful. This made sense to me because while AD doesn't care about the username's capitalization when it authenticates (hence why manually running `kinit` and typing the password worked), using a keytab file means that the wrong salt was given.
