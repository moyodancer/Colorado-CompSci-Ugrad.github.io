---
title: Help@CS INGInious
keywords: procedure, INGInious
sidebar: helpatcs_sidebar
permalink: helpatcs_procedures_inginious.html
folder: helpatcs
---

## Introduction
[INGInious](https://github.com/UCL-INGI/INGInious)

## Location
In Google Cloud: No Organization, Project CSEL

## Configuration

The inginious user hosts the application:

```
/home/inginious/inginious/configuration.yaml
```

## Starting (As the inginious user)

```
cd /home/inginious/inginious
./uwsgi-run-d.sh &
```

## Stopping (As the inginious user)

```
kill uwsgi
```
