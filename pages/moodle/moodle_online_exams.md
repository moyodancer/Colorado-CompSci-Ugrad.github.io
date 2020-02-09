---
title: Online Exams
keywords: moodle, wireless, online
sidebar: moodle_sidebar
permalink: moodle_online_exams.html
folder: moodle
---

# CS Department Wireless & Online Exams

Many classes taught in the CS department use online exams. Those exams depend on wireless networks that can support the students in the class accessing the exam system (typically Moodle). Several classrooms are unable to support that wireless use. This document describes the wireless network hardware that can be deployed in different classrooms. It also has suggestions on successfully using online exams.

## Suggestions for online exams

### Proper exam Settings
You should set your exam to use **unlimited** attempts and **each attempt builds on the previous**. These options are in the exam settings, and the option to build upon the prior attempt is not shown until you select unlimited attempts. Correct settings are shown in this image:

<img src="/images/online-exams/moodle-exam-build.png" width="300" />

These settings insure that if students are disconnected from the network or need to switch laptops they can continue to work on the same exam and don't need to start with a new exam.

### Planning the exam event

* Inform the CS EdTech staff that you'll be having an exam. They can scale the Moodle quiz system to handle your exam without having to wait for it to automatically scale up.
* Arrive early and set up the AP. It takes 2-3 minutes to plug things in and another 2 minutes for the networks to become available.
* Tell students the added network is called **csdept** and shouldn't have a password. Not all laptops will be able to see the extra network (see below) but the goal is to off-load as many students as possible from the campus networks.
* Have some spare chromebooks in case student laptops act up.

## Extra Wireless Network

Classrooms such as Humanities 1b50 have limited WiFi capabilities. Most of the problems encountered in online exams involved the number of students who can connect to the WiFi, not the network bandwidth of the available network.

The following shows the signal strength of different networks in HUMN 1b50 in Spring 2020 as captured by the Android application [NetSpot](https://play.google.com/store/apps/details?id=com.etwok.netspotapp&hl=en_US).

<img src="/images/online-exams/HUMN-1b50-low.png" width="150" /><img src="/images/online-exams/HUMN-1b50-mid.png" width="150" /><img src="/images/online-exams/HUMN-1b50-high.png" width="150" />

Note that the middle bands are largely empty. These bands have [special regulations because they may conflict with weather radar](https://en.wikipedia.org/wiki/List_of_WLAN_channels#5_GHz_or_5.8_GHz_(802.11a/h/j/n/ac/ax) ) and many older or consumer grade laptops won't use those bands. The department has a set of access points that can be configured to operate in those bands. In sample setups, at least 100 students were able to be moved to the supplemental network allowing the other students to use the more limited campus wifi in the room.

### Network setup

The network equipment consists of a [UniFI security gateway](https://www.ui.com/unifi-routing/usg/) (provides DHCP/NAT), multiple access points and a power-over-ethernet (PoE) switch and power source. The equipment needs a single input network connection. The access points are either [UniFi NanoHD](https://store.ui.com/products/unifi-nanohd-us) -- best -- or [UniFi AC Pro](https://www.ui.com/unifi/unifi-ap-ac-pro/) provided by Ben Shapiro. Each NanoHD can support ~150 students.

The devices are configured using a [UniFi software controller](https://sdr.cs.colorado.edu:8443). This software can be used to configure the access points to the specific room.

For example, looking at the images for HUMN 1b50, it makes sense to deploy one AP at channel 60 and another at channel 136. This avoids conflicts with the existing campus network. The EdTech staff will provide settings for each room and will set up equipment prior to your exam for that room.

## Information about different rooms

### Humanities 1b50

Humanities 1b50 seats 295 students but can only support 80-100 students taking an exam using the single OIT access point on the floor at the front of the room. Some wireless from other locations comes through, but not enough.

Based on survey in Spring 2020, it's recommend to use two NanoHD access points set to channel 60 and 136 resp. You should disable the 2.4Ghz radio because that band is already used by OIT wifi.

<img src="/images/online-exams/HUMN-1b50-low.png" width="150" /><img src="/images/online-exams/HUMN-1b50-mid.png" width="150" /><img src="/images/online-exams/HUMN-1b50-high.png" width="150" />

The lectern has a single 1G Ethernet jack and two power ports. Currently, there is a green ethernet cable but you should bring a spare.

### Fleming 
....Add information here as people survey the other rooms...
