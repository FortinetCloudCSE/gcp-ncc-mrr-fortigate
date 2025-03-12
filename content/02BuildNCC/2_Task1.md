---
title: "Task 1 - Create NCC Hub"
linkTitle: "NCC Hub"
weight: 1
---

## Create NCC Hub

1. From the console, type ``` Network Connectivity ``` in the search bar.  One of the first options should be the Network Connectivity Center.  Click on that to move forward.

     ![NCC Search](ncc_search.png)

1. From the Network Connectivity Center screen, select **HUBS > CREATE HUB**
     - Provide a Name
     - Preset Topology will be **Mesh topology**
     - Private Service Connect Propagation will be **Off**
     - Click on **Next Step**

     ![Hub Configuration](hub_conf.png)

1. Add two spokes to the Hub
     - Now Click on **Add A SPOKE**
     - Spoke type will be **Router appliance**
     - Provide a Name that will make sense when troubleshooting.
     - Region will be **us-central1 (Iowa)**
     - Select **On** for Site-to-site data transfer
     - for VPC, we will choose **routelab-ncc-vpc-random**
     - Click on **ADD INSTANCE**
     - In the drop down, choose the FortiGate.  It will be named **routelabe-fgt1-random**
     - Click **Done**

     ![New Spoke](new_spoke.png)

     - Now you will Click **Add A Spoke** once more to add the FortiGate in US-EAST1

     ![New Spoke2](new_spoke2.png)

     - Repeat the process, but this time for region, choose **us-east1 (SouthCarolina)**
     - Now you should have two spokes to be added to the Hub.  Click on **CREATE**

     ![Create 2](create2.png)

### Proceed to the next section
    