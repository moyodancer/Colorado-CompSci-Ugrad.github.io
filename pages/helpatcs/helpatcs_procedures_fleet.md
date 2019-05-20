---
title: Help@CS Fleet
keywords: procedure, fleet
sidebar: helpatcs_sidebar
permalink: helpatcs_procedures_fleet.html
folder: helpatcs
---

## Introduction

Fleet is a docker management system (like modern Kubernetes) that runs many of the CS underlying services

## Commands
```
fleetctl list-units
fleetctl list-unit-files
```

## When things go bad
```
systemctl restart fleet.service
```

## Connect to a container
```
docker ps
docker exec -it [CONTAINER_ID] /bin/bash
```
