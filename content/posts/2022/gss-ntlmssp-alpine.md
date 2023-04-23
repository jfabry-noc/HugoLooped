---
title: "Compiling GSS-NTLMSSP For Alpine Linux"
date: 2022-08-14T18:23:54-04:00
tags:
  - GSS-NTLMSSP
  - NTLM
  - Alpine
  - Linux
draft: false
---

I recently found myself working on a project which required the [GSS-NTLMSSP](https://github.com/gssapi/gss-ntlmssp) library. The application is going to be delivered via Kubernetes, so I needed to build a Docker image with this library. The [project's build instructions](https://github.com/gssapi/gss-ntlmssp/blob/main/BUILD.txt) are pretty clear about what the dependencies are, but given that this was going to be a Docker image, I wanted to use [Alpine Linux](https://www.alpinelinux.org/) as the base image in order to keep the image size as small as possible. The problem with this is that Alpine is just different enough to require different dependencies, publish their [packages](https://pkgs.alpinelinux.org/packages) under different names, etc.

I started off by just doing things manually where I fired up a local Docker container running the base [Docker image](https://hub.docker.com/_/alpine) and then manually installing each package via `apk add {package_name}` to make troubleshooting easier. Once I had all of the packages from the aforementioned build documentation, I ran `./configure`, looked at the errors, figured out which package was missing, installed it, and tried again. After several iterations of this process, `./configure` executed successfully and it was time to attempt running `make`.

`make` ran for a minute but then would error out with:

> undefined reference to 'libintl_dgettext'

This seemed odd to me because while running `./configure` I had received an error that `msgfmt` couldn't be found, and I had installed `gettext-dev` in order to accommodate that. After some additional packages searches, I discovered the `musl-libintl` library is also available. I attempted to install _that_ but received an error that it was attempting to modify a file controlled by the `gettext-dev` package. I uninstalled that via `apk del gettext-dev` and then ran into another error that--duh--`msgfmt` was now missing again. I handled that by just installing the vanilla `gettext` package, not the `-dev` version, and then **finally** everything compiled successfully.

The following is the full list of packages that I needed in order to get the build to succeed:

- autoconf
- automake
- build-base
- docbook-xsl
- doxygen
- findutils
- gettext
- git
- krb5-dev
- libwbclient
- libtool
- libxml2
- libxslt
- libunistring-dev
- m4
- musl-libintl
- pkgconfig
- openssl-dev
- samba-dev
- zlib-dev

Note that `git` is included just to clone the repo, and `build-base` is the meta package I used for compiling C software since just installing something like `gcc` will not include everything needed.
