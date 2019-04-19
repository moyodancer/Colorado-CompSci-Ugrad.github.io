---
title: Education OpenStack
keywords: procedure, openstack, education
sidebar: helpatcs_sidebar
permalink: helpatcs_procedures_edstack.html
folder: helpatcs
---

## Introduction

The CS Department has a OpenStack deployment that is used to support our educational mission.

## SSL Cert

The Education OpenStack uses certbot for SSL certificates. You must be on campus or using the VPN to connect to the server and renew the certificate.

```
ssh 172.20.66.52
certbot --apache renew
systemctl restart httpd
```

## Problem: VM network failure

When this happens, the simple resolution is to reboot the compute host. You will need to login to the openstack webpage and identify the troubled node. You will also want to take note of any VMs running on that server. SSH to the compute node, reboot it, and then restart any VMs that were running on the node.
