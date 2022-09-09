---
title: Computer Science Education Lab (CSEL) ELRA
keywords: elra, shell, csel
sidebar: helpatcs_sidebar
permalink: helpatcs_elra.html
folder: helpatcs
---

## Remote Access

All IdentiKey users on the CS career track have access this service.
SSH into `elra.cs.colorado.edu`, which consist of the following hosts:

- elra-01.cs.colorado.edu
- elra-02.cs.colorado.edu
- elra-03.cs.colorado.edu
- elra-04.cs.colorado.edu

You may set a shell globally on the [IdentiKey Portal](https://identikey.colorado.edu/choose_shell.html). This is `/bin/bash` by default.

To connect to the lab you must be on the campus network or [connected to VPN via the AnyConnect Client](https://oit.colorado.edu/services/network-internet-services/vpn). Then, you can ssh into one of the above hosts via:
```
ssh <IdentiKey>@<host>
```

## Websites
A personal site can be hosted if you create `~/public_html` or `~/.www` on any one of of the elra hosts mentioned above. 
[https://csel-web.cs.colorado.edu/~identikey](https://csel-web.cs.colorado.edu/~identikey).

If you are having trouble (ie permission denied issues), make sure the web server has permission to read this
directory.
```
$ chmod o+x ~
$ chmod o+rx ~/.www
```
