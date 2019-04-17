---
title: Proxying Web Applications
keywords: Coding, Environment, web, app
sidebar: coding_environment_sidebar
permalink: coding_environment_proxy.html
folder: coding_environment
---

You may occasionally want to create a web application that you or
other can access through a web browser. This is supported in the
coding environment using a *proxy* mechanism.

There are two ways this proxy mechanism is used. In certain applications,
such as the Visual Studio Code plugin, the proxy mechanism has been setup
in advance to use a named end-point. 

For your application, you'll need to know your username and the TCP port
you are attempting to connect to. For example, if your usename is **XYZZY**
and you're attempting to connect to port **9999**, you would open a
web browser for the url `https://coding.csel.io/user/XYZZY/proxy/9999`.

This can be useful if you're prototyping applications using simple web
frameworks such as Flask, developing dashboards using [Plotly
"Dash"](https://plot.ly/products/dash/) or using R-Studio.


## Launching Visual Studio Code

[![visual studio code launching](https://img.youtube.com/vi/zwmS3yvfpV8/0.jpg)](https://www.youtube.com/watch?v=zwmS3yvfpV8)

