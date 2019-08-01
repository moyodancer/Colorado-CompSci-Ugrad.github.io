---
title: Help@CS Icinga
keywords: procedure, icinga, icinga2
sidebar: helpatcs_sidebar
permalink: helpatcs_procedures_icinga.html
folder: helpatcs
---

## Introduction
[Icinga2](https://icinga.com/) monitors availability and performance, gives you simple access to relevant data and raises alerts to keep you in the loop

## Network Setup
You must use the Education Stack VPN to connect to this service. The login and configuration information can be found in the CS Department vault.

## Location
```
SSH to 172.20.66.20

sudo docker exec -it 0ff5b5a3fa87 bash

/etc/icinga2/icinga2.conf
/etc/icinga2/zones.d/global/templates.conf
/etc/icinga2/zones.d/global/services.conf (ignore where host.vars.noclient == "true")
/etc/icinga2/zones.d/global/notifications.conf (ignore where match("epic-*", host.name)
apply Notification "mail-host-notification" to Host {
  import "mail-host-notification"

  user_groups = [ "epic-admin" ]

  interval = 24h
  times.begin = 5m

  //assign where host.vars.notification.mail
  assign where match("epic-*", host.name)
}

docker ps
docker exec -it [CONTAINTER_ID] /bin/bash
```

Configuration files are:
```
/var/lib/icinga2
/etc/icinga2
```

Validate config file:
```
icinga2 daemon -C
```
