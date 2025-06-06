---
title: "Task 3 - Verify Routing"
linkTitle: "Verify Routes"
chapter: false
weight: 3
---

## Check NCC VPC Routing Table

1. Now that we have the spoke configured, we need to go back to the Application project and delete the default route which is configuredfor the peer-vpc This route will take precedence over the default being advertised by FortiGate.
    - Navigate to VPC Network > Routes > Route Management
    - select the box next to the default route assosicated with the peer networks.
    - Click **Delete**

    ![Delete route](delete_route.png)

1. Verify routing in the peer vpc.
    - Click on the Effective routes tab and chose the peer vpcs for region **us-central1** and click **view**
    - Verify that we see the route from the Remote FortiGate as well as the default pointing to our NCC vpc.

    ![eff1 routes](eff-routes1.png)


1. Verify routing in the FortiGate.
    - Access FortiGate CLI and verify that we see **10.18.0.0/24**, **10.19.0.0/24**, **10.20.0.0/24** and **10.21.0.0/24**

```sh
fgt1 # get router info bgp summary

VRF 0 BGP router identifier 10.15.0.3, local AS number 65200
BGP table version is 4
2 BGP AS-PATH entries
0 BGP community entries

Neighbor    V         AS MsgRcvd MsgSent   TblVer  InQ OutQ Up/Down  State/PfxRcd
10.15.0.252 4      65100     189     221        4    0    0 01:00:56        6
10.15.0.253 4      65100     189     212        2    0    0 01:00:57        6
10.17.1.1   4      65200      65      70        4    0    0 00:55:29        1

Total number of neighbors 3


fgt1 # get router info routing-table bgp
Routing table for VRF=0
B       10.16.0.0/24 [20/333] via 10.15.0.253 (recursive via 10.15.0.1, port2), 01:01:06, [1/0]
B       10.18.0.0/24 [20/100] via 10.15.0.253 (recursive via 10.15.0.1, port2), 00:04:50, [1/0]
B       10.19.0.0/24 [20/333] via 10.15.0.253 (recursive via 10.15.0.1, port2), 00:04:50, [1/0]
B       10.20.0.0/24 [20/100] via 10.15.0.253 (recursive via 10.15.0.1, port2), 00:02:55, [1/0]
B       10.21.0.0/24 [20/333] via 10.15.0.253 (recursive via 10.15.0.1, port2), 00:02:55, [1/0]
B       192.168.100.0/24 [200/0] via 10.17.1.1 (recursive via RMT-FGT tunnel 34.121.250.35), 00:55:15, [1/0]


fgt1 # 
```
