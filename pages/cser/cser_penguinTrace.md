---
title: penguinTrace
keywords: penguine, trace, debug, gdb
summary: "TpenguinTrace allows you to see the instructions that code compiles down to and see how they get executed in order to run a program."
sidebar: mydoc_sidebar
permalink: cser_penguinTrace.html
folder: cser
---

## Introduction
penguinTrace allows you to see the instructions that code compiles down to and see how they get executed in order to run a program. penguinTrace starts a web-server, and then code can be entered into the web interface. After compiling the code (▶), the interface will show the source code on the left and the assembly on the right. The current line of code will be highlighted on both sides.


## Install Docker
If you do not already have Docker installed, please follow the directions from the [Docker website to install Docker](https://docs.docker.com/install/#supported-platforms) on your personal computer.

## Install penguinTrace
Once Docker has been installed, open a Terminal window on your computer, and run the following command:
```
docker build -t penguinTrace github.com/penguintrace/penguintrace
```

## Running penguinTrace
With Docker and penguinTrace installed, open a terminal window and run the following command:
```
docker run -it -p 127.0.0.1:8080:8080 --tmpfs /tmp:exec --cap-add=SYS_PTRACE --cap-add=SYS_ADMIN --rm --security-opt apparmor=unconfined penguintrace penguintrace
```

## Using penguinTrace
Once you have started penguinTrace, simply point your web browser to [127.0.0.1:8080](http://127.0.0.1:8080/)


## Managing penguinTrace
In the terminal window, you can use the following keys to control the running container
```
 Commands:
  q - Shutdown server
  l - List active sessions
  k - Kill all active sessions
```
