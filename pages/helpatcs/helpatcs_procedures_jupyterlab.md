---
title: JupyterLab
keywords: procedure, openstack, education
sidebar: helpatcs_sidebar
permalink: helpatcs_procedures_jupyterlab.html
folder: helpatcs
---

## Problem: Gateway timeout
## Problem: Website responds, but cannot start user environment

Situation - You are able to login, but the user container is unable to start. You may notice that the startup log (in the web browser) repeatedly attempts to download the same image.

Resolution - Restart the 'proxy' pod

```
kubectl config use-context gke_emerald-agility-749_us-west1-a_jupyterhub-campus
kubectl config set-context gke_emerald-agility-749_us-west1-a_jupyterhub-campus --namespace jupyterhub-campus
kubectl get pod | grep proxy
kubectl delete pod [proxy pod name]
```

## Problem: Student container inaccessible

Login to the [Hub Admin](https://coding.csel.io/hub/admin) and stop the student's container
