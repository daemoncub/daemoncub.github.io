---
layout: post
title: "MAC Flooding Lab"
date: 2026-03-04 13:10:00 +0300
categories: security
tags: pentesting networking mac-flooding
---

### MAC flooding lab

Hey there, I made this lab for my pentesting lab assignment as part of the course in my major as an information security senior student.

In this lab MAC flooding attack will be demonstrated targeting the switch so we're operating on layer 2 of the OSI model the data link layer.

On windows setting up the simulation environment using GNS3, you can find tutorials on how to get things started on YT. In short i used GNS3 GUI, GNS3 VM, kali VM — on VMware hypervisor — and Cisco switch image file as the structuring parts of the environment.

The network consisting of kali virtual machine (attacker), PC1, PC2 and a switch it will be connected like shown below.

![](/assets/images/mac-flooding-lab/network-setup.png)

GNS3 GUI in windows

![](/assets/images/mac-flooding-lab/gns3-gui.png)

Topology

I configured the switch, anyway our concern is the mac address table — that is stored in CAM (content addressable memory) — shown below…

![](/assets/images/mac-flooding-lab/switch-terminal.png)

Switch terminal in enable mode

In this lab our switch does not restrict every port to a single MAC address nor applies any security policy. This insecure setup will allow attack attempt to succeed.

For context i added a new virtual network adapter in VMware settings that will connect VMs with GNS3. To ensure the connection between Kali VM and GNS3 VM, Let's view the IP configuration in both VMs ..

![](/assets/images/mac-flooding-lab/gns3-vm-ip-config.png)

IP configurations in GNS3 VM

![](/assets/images/mac-flooding-lab/kali-vm-ip-config.png)

IP configurations in kali VM

They are both connected to the same network adapter `eth0`, and sending a ping request from kali to the other VM was received successfully. this is part of the lab setup and essential to connect kali VM to GNS3 simulation environment.

Now that everything is ready, let's start the attack shall we …

![](/assets/images/mac-flooding-lab/attacker-machine.png)

In the attacker machine launching attack

In the first attack we will be using `macof` command specifying `192.168.0.1` the gateway of `vlan1` which is where the switch receives requests to assign MAC addresses to physical interfaces (ports) such as `g0/0, g0/1.. etc.`

What it does is basically filling the CAM table — of limited memory — with a huge number of fake MAC addresses until it no more act as a proper switch, Instead broadcasts all received traffic to all ports just like a `hub.`

Let's view the CAM table one more time…

![](/assets/images/mac-flooding-lab/mac-address-table.png)

Switch terminal showing MAC addresses assigned to interfaces.

This is called MAC flooding, now as the attacker is part of the network all the traffic is arriving at their doorstep, potentially causing sniffing attack..

I hope this was useful, see you later.