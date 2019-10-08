---
title: Education OpenStack
keywords: procedure, openstack, education
sidebar: helpatcs_sidebar
permalink: helpatcs_procedures_edstack.html
folder: helpatcs
---

## Introduction

The CS Department has a OpenStack deployment that is used to support our educational mission.

## Command Line Tools (macOS)
As with the gcloud command-line tool, Openstack command-line clients require python 2.7 or later, but below python 3.

CLI tools are required for the proper administration and management of Openstack. They are recommended for cloud end-users as well.

OpenStack provides two sets of client tools, the [unified client](https://docs.openstack.org/python-openstackclient/latest/), and [individual clients](https://docs.openstack.org/newton/user-guide/common/cli-overview.html). The unified client provides support to most of the OpenStack common services. It is benficial as a goal of this project is consistent naming for commands and arguments. It however lacks some services such as `neutron`. 

It is recommended to use the unified client. Individual clients are deprecated. It is only recommened to install individual clients if the unified client does not yet support the service to be managed.

Install `OpenStackClient` aka `OSC` aka Unified Client.

~~~
# Install pip if required
easy_install pip

# Install the OSC
pip install python-openstackclient
~~~

If an individual client needs installed, such as `neutron`:

~~~
pip install python-neutronclient
~~~

The full table of individual clients can be viewed [here](https://docs.openstack.org/newton/user-guide/common/cli-overview.html)

### Source the `openstack.cs.colorado.edu` RC file.
Login to CSEL Openstack, and download the RC file from the `API Access` tab from the compute [access and security](https://openstack.cs.colorado.edu/project/access_and_security/) page.

Source your RC file and enter your OpenStack credentials.

~~~
source ~/Downloads/csel-openrc.sh
~~~

Test the client, in this example, list compute services.

~~~
openstack compute service list

engr2-1-3-220-edu:~ wazh7587$ openstack compute service list
+----+------------------+--------------------+----------+----------+-------+----------------------------+
| ID | Binary           | Host               | Zone     | Status   | State | Updated At                 |
+----+------------------+--------------------+----------+----------+-------+----------------------------+
|  1 | nova-console     | openstack.csel.loc | internal | enabled  | up    | 2019-10-08T15:51:59.000000 |
|  2 | nova-scheduler   | openstack.csel.loc | internal | enabled  | up    | 2019-10-08T15:52:03.000000 |
|  3 | nova-conductor   | openstack.csel.loc | internal | enabled  | up    | 2019-10-08T15:52:04.000000 |
|  4 | nova-cert        | openstack.csel.loc | internal | enabled  | up    | 2019-10-08T15:52:02.000000 |
|  5 | nova-compute     | smg-1.csel.loc     | nova     | enabled  | up    | 2019-10-08T15:52:04.000000 |
|  6 | nova-compute     | r900-11.csel.loc   | nova     | enabled  | up    | 2019-10-08T15:51:59.000000 |
|  7 | nova-compute     | r900-13.csel.loc   | nova     | enabled  | up    | 2019-10-08T15:52:00.000000 |
|  8 | nova-compute     | r900-12.csel.loc   | nova     | enabled  | up    | 2019-10-08T15:52:03.000000 |
|  9 | nova-compute     | sm1-2.csel.loc     | nova     | enabled  | up    | 2019-10-08T15:52:03.000000 |
| 10 | nova-compute     | sm1-4.csel.loc     | nova     | enabled  | up    | 2019-10-08T15:51:56.000000 |
| 11 | nova-compute     | sm1-8.csel.loc     | nova     | enabled  | up    | 2019-10-08T15:51:59.000000 |
| 12 | nova-compute     | sm1-6.csel.loc     | nova     | enabled  | up    | 2019-10-08T15:51:58.000000 |
| 13 | nova-compute     | sm1-7.csel.loc     | nova     | disabled | down  | 2017-08-11T17:27:30.000000 |
| 15 | nova-compute     | sm1-3.csel.loc     | nova     | enabled  | up    | 2019-10-08T15:51:56.000000 |
| 16 | nova-compute     | sm1-1.csel.loc     | nova     | enabled  | up    | 2019-10-08T15:52:05.000000 |
| 17 | nova-compute     | sm1-5.csel.loc     | nova     | enabled  | up    | 2019-10-08T15:51:57.000000 |
| 19 | nova-consoleauth | openstack.csel.loc | internal | enabled  | up    | 2019-10-08T15:52:05.000000 |
+----+------------------+--------------------+----------+----------+-------+----------------------------+
~~~

To enter interactive mode, simply run `openstack`.

## SSL Cert

The Education OpenStack uses certbot for SSL certificates. You must be on campus or using the VPN to connect to the server and renew the certificate.

```
ssh 172.20.66.52
certbot --apache renew
systemctl restart httpd
```

## Problem: VM network failure

When this happens, the simple resolution is to reboot the compute host. You will need to login to the openstack webpage and identify the troubled node. You will also want to take note of any VMs running on that server. SSH to the compute node, reboot it, and then restart any VMs that were running on the node.
