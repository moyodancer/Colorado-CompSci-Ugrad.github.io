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


## Scaling Preemptive nodes

1. SSH into the csel-backup server (You must be connected to the CU VPN)
```
ssh 35.233.171.80
```
OR
```
gcloud config set project emerald-agility-749
gcloud compute ssh csel-backup
```

2. Switch to the csel user
```
sudo su - csel
```

3. Edit the scaling environment script
```
vi /home/csel/gcloud-scripts/scale-cluster/ClusterEnv.sh
```

4. Adjust MAX_SCALE_SIZE (Minimum 3. Maximum 8+)
```
MAX_SCALE_SIZE=10
```

## Email users of PVC Claims / Persistent Storage

1. Setup the session environment
```
gcloud config set project emerald-agility-749
kubectl config use-context gke_emerald-agility-749_us-west1-a_jupyterhub-campus
kubectl config set-context gke_emerald-agility-749_us-west1-a_jupyterhub-campus --namespace jupyterhub-campus
```

2. Generate mailing list
{% include note.html content="The year/month must be changed to match the current year and month so that new PCV claims are ignored. This ensures that users who have just started using the Coding Environment are not included in the email list" %}
```
NOTIFY=$( kubectl get pv -o custom-columns=NAME:.spec.claimRef.name --no-headers=true | grep claim | awk '{split($0,a,"-"); printf("%s%s", a[2], "@colorado.edu,")}' )
for user in `kubectl get pv -o custom-columns=CREATED:.metadata.creationTimestamp,NAME:.spec.claimRef.name --no-headers=true | grep claim | grep 2019\-05`
do
  NOTIFY=`echo $NOTIFY | sed "s/\b$user\b//g"`
done
echo $NOTIFY
```

{:start="3"}
3. Send email to users
{% include note.html content="Be sure to include the list of email addresses in the Blind Carbon Copy (BCC) of the email and not the To or Carbon Copy (CC) to prevent the mailing list from being shared and/or responses to the email going to all users." %}
```
Hello Coding Environment User,

It is that time of year again where we remove old persistent storage from the Coding Environment (coding.csel.io). You have been identified as a user of the system that has active storage on the system. In order to prevent the loss of any or your work, please login and backup your data before May 29th, 2019.

All persistent storage will be removed at that time and any data not backed up will be lost. No action is required if you have already backed up your data or have nothing there that needs to be retained. When you start using the Coding Environment again, a new 2GB storage will be created automatically for the new semester. If you need to keep your persistent storage you can request an extension using this Google Form.

Please contact help@cs.colorado.edu with any questions or concerns.

Thank you.


CSEL EdTech Services Team
```

## SSL Certificate Expired/Expiring

```
gcloud config set project emerald-agility-749
kubectl config set-context gke_emerald-agility-749_us-west1-a_jupyterhub-campus --namespace jupyterhub-campus
kubectl get pod | grep autohttp
kubectl delete pod [autohttps pod name]
```
