---
title: "kubectl logs Symbolic Link Error"
date: 2022-09-10T18:27:23-04:00
tags:
  - kubectl
  - Kubernetes
  - symbolic link
draft: false
---

I recently ran across an interesting error with my development Kubernetes cluster, and while I still have no idea what I may have done to cause it, I at least figured out how to rectify it. As is commonly the case, most of the things I end up deploying to Kubernetes simply log to standard out so that I can view logs with the `kubectl logs` command. While running this against a particular deployment, though, I received an error:

> failed to try resolving symlinks

Looking at the details of the error message, it seemed that running a command like:

```shell
kubectl logs -f -n {namespace} {podname}
```

Is looking for a symbolic link at the following path:

**/var/log/pods/{namespace}_{pod-uuid}/{namespace}**

The end file itself seems to be something extremely simple, like a number followed by a `.log` suffix. In my case, it was **4.log**. That symbolic link _then_ points to a file at:

**/var/lib/docker/containers/{uuid}/{uuid}-json.log**

Where the **uuid** is the UUID of the container in question.

**Note:** The directory above isn’t even viewable without being root, so depending on your setup you may need to use `sudo ls` to be able to look at what’s there.

I was able to open the **-json.log** file and validate that it had the information I needed, so I just had to create the missing symlink. I did that with:

```shell
sudo ln -s /var/lib/docker/containers/{uuid}/{uuid}-json.log 4.log
```

Since my shell was already in the **/var/log/pods/{namespace}_{pod-uuid}/{namespace}** directory, I didn’t need to give the full path to the actual link location, just specify the relative file of **4.log**.

Sure enough, after creating this I was able to successfully run `kubectl logs` against the previously broken pod.
