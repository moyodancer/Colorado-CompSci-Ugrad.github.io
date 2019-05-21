---
title: ELRA
keywords: elra, shell, csel, Computer Science Education Lab
sidebar: helpatcs_sidebar
permalink: helpatcs_elra.html
folder: helpatcs
---

## Remote Access

You may access the lab remotely via SSH (SCP, SFTP) using your
[IdentiKey](http://www.colorado.edu/oit/services/identity-access-management/identikey)
and password. The current remote access machines are:

- elra-01.cs.colorado.edu
- elra-02.cs.colorado.edu
- elra-03.cs.colorado.edu
- elra-04.cs.colorado.edu


## Websites
You have a student website hosted at
[csel-web.cs.colorado.edu/~identikey](https://csel-web.cs.colorado.edu/#). This
site can be located under either `~/public_html` or `~/.www`. If you are
having trouble, make sure the web server has permission to read this
directory.
```
$ chmod o+x ~
$ chmod o+rx ~/.www
```

## Passwords

By default, your password for CS Department education systems is your identikey
password. This password is stored by OIT and is the same password that you use
to access your email, mycuinfo, etc. Hence, if you are having password problems
in the lab test against one of those systems.

You can change your password using the `csel-passwd` tool on the
desktops on campus or the elras.

	usage: csel-passwd set|reset|test [IDENTIKEY]

  	  This tool is for changing your CSEL password. If not specified, IDENTIKEY defaults
  	  to your current username ($USER).

  	  set      Set a local CSEL password (will not reflect elsewhere on campus)
  	  reset    Reset back to campus identikey password
  	  test     Test your current password

For example, to change to a local password

	$ csel-passwd set

To go back to the default and defer to your identikey password

	$ csel-passwd reset

[slides](https://docs.google.com/presentation/d/1tsrShNRlQUFrnuPoZySgK1gHneMaQtBHXXkvZ9j3sno/edit?usp=sharing)
