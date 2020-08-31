---
title: CU CS Virtual Machine
keywords: vm, vmware, cu, cs
summary: "The CU CS VM provides a free, local Linux-based development system similar to what we run in our labs. It is primarily used in CS undergraduate courses to ensure all students, teachers, and TAs are using the same standardized environment."
sidebar: mydoc_sidebar
permalink: cser_cs_vm.html
folder: cser
---

{% include note.html content="The current VM version is CU CS VM Fall 2020" %}

{% include note.html content="Use of the CU CS VM is dependent on VMware or VirtualBox. Please complete the [VMware](/cser_vmware.html) setup before proceeding." %}

## System Requirements

To run smoothly, the VM requires a minimum of:

* 8GB of RAM
* Intel or AMD processor with virtualization support (VT-x or AMD-V)
* 4 physical processor cores
* 32GB of free HD space (64GB+ if doing kernel compilation exercises)
* An [OIT recommended Operating System](https://oit.colorado.edu/software-hardware/software-lifecycle) (Currently Windows 10 1909, or macOS 10.15 Catalina)
* Either VMWare Fusion 10+ (macOS) or Workstation 14+ (Windows)
* VirtualBox 6.1+ if you are a native Linux user (See FAQ for different download)

## Download

[CU CS Virtual Machine Fall 2020](https://foundation.cs.colorado.edu/vm/CU-CS-VM-Fall-2020.zip) (4.18 GiB)

If you are experiencing difficulty either downloading or installing the VM, please send a message to Computer Science IT Services at [helpcs@colorado.edu](mailto:helpcs@colorado.edu) to open a ServiceNow case.

## Importing the Virtual Machine (macOS)

1) Extract the downloaded archive to expand the `CU CS Virtual Machine Fall 2020.vmwarevm` directory   
2) Move `CU CS Virtual Machine Fall 2020.vmwarevm` to `/Users/<yourUserAccount>/Virtual Machines`  
3) Open the bundle  
4) You will recieve a dialog asking if the machine was moved or copied, click `I Copied It`  
5) The Virtual Machine will then boot to the Welcome Screen  

## Importing the Virtual Machine (Windows)

1) Extract the downloaded archive to expand the `CU CS Virtual Machine Fall 2020.vmwarevm` directory  
2) Move the extracted `CU CS Virtual Machine Fall 2020.vmwarevm` directory to `C:\Users\<yourUserAccount>\Documents\Virtual Machines\`  
3) Open VMWare Workstation 15 Pro  
4) On the "Home" tab, click "Open a Virtual Machine"  
5) Navigate into the `CU CS Virtual Machine Fall 2020.vmwarevm` directory and double click `CU CS VM Fall 2020.vmx`  
6) An entry `CU CS Virtual Machine Fall 2020` will now be present  
7) You will recieve a dialog asking if the machine was moved or copied, click `I Copied It`  
8) Power on the Virtual Machine to boot to the Welcome Screen  

## Usage

The VM runs a minimal installation of Ubuntu 16.04.7 LTS (Xenial) with preinstalled binaries for certian CSCI courses. 16.04.7 is required for some CSCI courses related to compiling older kernel source. In addition, it contains time series analysis tools and a VSCode environment. Use this VM if you cannot utilize the cloud coding environment due to poor or unavailable internet connectivity.

### Basics

To launch the VM, select it in the left-side VMWare VM list and either double-click or select the green Start button at the top of the VirtualBox window. The VM will launch in its own window. After completing the boot process (you may see a black screen with scrolling text for a few moments), the VM will present a Welcome screen for you to create your account. Enter a username and password of your choosing for this.

VMWare Guest Tools are pre-installed, so you are able to:
* Resize the VM window: the guest system will adjust resolution to accommodate your desired window size
* Copy and Paste / Drag and Drop items from your machine to the virtual machine

## Frequently Asked Questions (FAQ)

### What is the root account password?

As on most Debian-based systems, the root account does not have a password set and is disabled. Use `sudo` if you need to run privileged commands on the account you created when you first boot the machine.

### May I use VirtualBox instead of Fusion/Workstation?

Yes, but please use the following link instead as it contains specific EFI firmware settings (nvram), as well as Guest Tools for VirtualBox

[CU CS Virtual Machine Fall 2020 VirtualBox](https://foundation.cs.colorado.edu/vm/VirtualBox/CU-CS-VM-Fall-2020.zip) (4.28 GiB)

### May I use another hypervisor?

Yes, but this is outside our support scope. It is also likley outside the support scope at the course level. 
The vhdx is EFI based, so you will need to add an efi boot option ((grubx64.efi / shimx64.efi) from the ESP) in your hypervisor firmware settings. It is highly recommended to instead use what your course recommends.

### Performance on the VM is poor, why is this?
Virtualization Support is likley disabled on your computer. Please ensure Virtualization Support is enabled in your computer's firmware settings.

### Why is networking not working on the VM?

The VM is configured by default to share your host computer network. Please ensure your host has network access.
If you have accidently modified your Virtual Network Adapter, please set it back to "Internet Sharing".

### Is there a way to make file access easier than drag and drop?

You can enable Shared Folders. For VMWare:

On macOS:  
1) Open Virtual Machine -> Settings  
2) Open Sharing  
3) Add a host folder you would like to Share to the VM  
4) Reboot the Virtual Machine  
5) You can then browse to this directory in the Ubuntu file manager by opening "Files", clicking "Other Locations", and navigating to `/mnt/hgfs/<yourFolder>`  

On Windows:  
1) Right Click "CU CS VM Fall 2020"  
2) Open Settings and Navigate to the Options Tab  
3) Click "Shared Folders"  
3) Add a host folder you would like to Share to the VM  
4) Reboot the Virtual Machine  
5) You can then browse to this directory in the Ubuntu file manager by opening "Files", clicking "Other Locations" -> "Computer", and navigating to `/mnt/hgfs/<yourFolder>`  

{% include links.html %}
