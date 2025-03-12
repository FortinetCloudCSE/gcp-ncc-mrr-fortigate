---
title: "GCP Advanced Routing with Network Connectivity Center"
linkTitle: "NCC Advanced Routing"
weight: 1
archetype: "home"
description: "GCP Advanced Routing with Network Connectivity Center"
---

This site will discuss how Google Cloud Platform (GCP) uses Network Connectivity Center (NCC) to provide dynamic routing.  Specifically, FortiGate will serve as a Networked Virtual Appliance (NVA) to provide routing and security between VPCs in CGP, as well as provide inter-regional HA. Students will use a virtual lab created in [Qwiklabs](https://fortinet.qwiklabs.com).  Each student will have their own project and resources.  This lab is allocated for 90 minutes.  However, the timer is set for 120 minutes in order to give students a bit of extra time, if needed.

### Learning Objectives
- Configure GCP NCC Hub and Spoke Architecture
- Configure FortiGate BGP peering with GCP NCC spokes
- Configure FortiGate overlay IPSec and BGP Peering between "gcp" and "remote" devices.
- Configure FortiGate Firewall Policy to allow and inspect traffic
 
### Concept
The below diagram shows the (High Level) architecture we are building for the lab. The remote FortiGate is represented as a VM, because the whole envrionment is built in GCP, but this could just as easily be an on-prem physical FortiGate or even FortiSASE.

![NCC High Level](gcp_ncc.svg)
