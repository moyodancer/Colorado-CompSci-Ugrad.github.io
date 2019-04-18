---
title: Coding Systems Implementation
keywords: Coding, Environment, implementation
sidebar: coding_environment_sidebar
permalink: coding_environment_implementation.html
folder: coding_environment
---

## Implementation

The coding environment is based on
[JupyterHub](https://jupyter.org/hub) using the
[Zero-to-JupyterHub (Z2JH)](https://zero-to-jupyterhub.readthedocs.io/en/latest/)
deployment model. The system is deployed on [Google Kubernetes Engine
(GKE) using Kubernetes](https://cloud.google.com/kubernetes-engine/).

The Z2JH deployment uses [the Helm package manager](https://helm.sh/)
to deploy most of the components using standard releases and version
numbers of those components. The container that defines the presented
coding environment are built using a custom Dockerfile [in a CS
department bitbucket
repository](https://bitbucket.org/ucbcsops/jupyterhub-helm-campus/src/master/).
That repository contains confidential information and isn't public.

That specification is based on the [the datascience docker-stacks
image](https://github.com/jupyter/docker-stacks) but has no dependence
on that docker image to avoid inadvertantly breaking dependences.
That image is extended with a set of additional programming environments.
The specific Dockerfile contains no confidential information.

## Scaling

The Z2JH environment is scaled across two node types. A small number of persistent
nodes are used to insure continuity of operations. Individual pods started by users
are deployed using pre-emptive nodes to reduce the cost of operations (preemptive nodes
are about 1/5th the code of persistent nodes).

Those pre-emptive nodes are automatically restarted every 24 hours,
which may cause disruption.  While the base Z2JH deployment will
"auto-scale" additional nodes to adjust to increased usage, the
process of deploying new nodes is fairly slow because the (large)
docker images need to be pulled over to the new nodes.

Thus, we deploy a `cron` script that adjusts the minimum number of
nodes in the pre-emptive pool early in the morning and reduces the
minimum in the evening. By "pre-scaling" our environment, we avoid
delays when *e.g.* 200+ students login at the start of a class.

## Associated repositories and source

The full deployment details are contains [in a CS department bitbucket repository](https://bitbucket.org/ucbcsops/jupyterhub-helm-campus/src/master/).

The VScoder extension uses binary releases [from the coder.com/code-server project](https://github.com/codercom/code-server). The extension is made visible on the front page using a [custom jupyter_proxy extension](https://github.com/dirkcgrunwald/jupyter_codeserver_proxy-) packages as a [(test) pypi package](https://test.pypi.org/project/jupyter-codeserver-proxy/).

