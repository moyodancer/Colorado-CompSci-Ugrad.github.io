---
title: CU CS Virtual Machine
keywords: vm, vmware, cu, cs
summary: "The CU CS VM provides a free, local Linux-based development system similar to what we run in our labs. It is primarily used in CS undergraduate courses to ensure all students, teachers, and TAs are using the same standardized environment."
sidebar: mydoc_sidebar
permalink: cser_cs_vm.html
folder: cser
---

{% include note.html content="The use of the CU CS VM is deprecated in favor of using the <a href='/coding_environment_landing_page.html'>Cloud Coding Environment. It should only be used if your Instructor requires it or if you do not have a stable internet connection.</a>" %}

{% include note.html content="The current VM version is CU CS VM Fall 2020" %}

{% include note.html content="Use of the CU CS VM is dependent on VMware. Please complete the [VMware](/cser_vmware.html) setup before proceeding." %}

## System Requirements

To run smoothly, the VM requires a minimum of:

* 8GB of RAM
* Intel or AMD processor with virtualization support (VT-x or AMD-V)
* 4 physical processor cores
* 32GB of free HD space
* An [OIT recommended Operating System](https://oit.colorado.edu/software-hardware/software-lifecycle) (Currently Windows 10 1909, or macOS 10.15 Catalina)

## Download

[HTTP](https://foundation.cs.colorado.edu/vm/CU-CS-VM-Fall-2020.zip) (5.87GB)

If you are experiencing difficulty either downloading or installing the VM, please email the Computer Science IT Services at [helpcs@colorado.edu](mailto:helpcs@colorado.edu).

## Importing the Virtual Machine (macOS)

1) Extract the downloaded archive to expand the `CU CS Virtual Machine Fall 2020.vmwarevm` folder
2) Open the bundle
3) You will recieve a dialog asking if the machine was moved or copied, click `I Copied It`
4) The Virtual Machine will then boot to the Welcome Screen

## Install (Windows)

1) Extract the downloaded archive to expand the `CU CS Virtual Machine Fall 2020.vmwarevm` folder

## Usage

The VM runs a minimal installation of Ubuntu 20.04 (Focal Fossa) with preinstalled binaries for certian CSCI courses. This consists of a VSCode environment, a gcc enivronment, time series analysis tools, etc...

### Basics

To launch the VM, select it in the left-side VMWare VM list and either double-click or select the green Start button at the top of the VirtualBox window. The VM will launch in its own window. After completing the boot process (you may see a black screen with scrolling text for a few moments), the VM will present a Welcome screen for you to create your account. Enter a username and password of your choosing for this.

Guest Tools are pre-installed, so you are able to:
* Resize the VM window: the desktop will resize itself to accommodate your desired window size
* Copy and Paste / Drag and Drop items from your machine to the virtual machine

## Frequently Asked Questions (FAQ)

### What is the root account password?

As on most Debian-based system, the root account does not have a password set and is disabled. Use `sudo` if you need to run privileged commands on the account you created when you first boot the machine.

### Performance on the VM is very bad, why is this?
Virtualization Support is likley disabled on your computer. Please ensure Virtualization Support is enabled in your computer's firmware settings.

### Why is networking not working on the VM?

The VM is configured by default to share your host computer network. Please ensure your host has network access.
If you have accidently modified your Virtual Network Adapter, please set it back to "Internet Sharing".

### Is there a way to make file access easier than drag and drop?

You can enable Shared Folders in the Virtual Machine Settings. To do so:

1) Open Virtual Machine -> Settings
2) Open Sharing
3) Add a host folder you would like to Share to the VM
4) You can then browse to this directory in the Ubuntu file manager by opening "Files", clicking "Other Locations", and navigating to `/mnt/hgfs/<yourFolder>`

{% include links.html %}
