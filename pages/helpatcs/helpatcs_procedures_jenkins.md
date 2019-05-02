---
title: Help@CS Jenkins CI
keywords: procedure, jenkins, ci
sidebar: helpatcs_sidebar
permalink: helpatcs_procedures_jenkins.html
folder: helpatcs
---

## Introduction

Help@CS runs an internal Jenkins CI server to automate the building and deployment of Moodle and other services.

## Links

[Help@CS Jenkins](https://ci.csel.io) (Must be on the CU VPN)

## Problem: Connection Refused

1. SSH into ci.csel.io
```
ssh ci.csel.io
```

2. Change to root user
```
sudo su -
```

3. Change to user colemat
```
su - colemat
```

4. Start the Jenkins container
```
cd /compose/jenkins
./run
```


## Problem: SSL certificate expired

1. SSH into ci.csel.io
```
ssh ci.csel.io
```

2. Change to root user
```
sudo su -
```

3. Change to user colemat
```
su - colemat
```

4. Start the Jenkins container
```
docker run -it --rm       -v certbot:/etc/letsencrypt       certbot/certbot       certonly       --webroot --webroot-path=/etc/letsencrypt/webroot/ci.csel.io
```

5. Provide the FQDN when prompted
```
ci.csel.io
```
