---
title: "Task 2 - Accept Application Spoke in NCC Hub"
linkTitle: "NCC Accept Peer"
chapter: false
weight: 2
---

|                            |    |  
|----------------------------| ----
| **Goal**                   | Accept proposed spokes
| **Task**                   | Log into NCC project and accept proposed spokes
| **Verify task completion** | You should have a total of four Active spokes in the NCC projects

## Configure Application Spoke in NCC Network

Navigate back to to the **original** NCC project 

1. Navigate to Network Connectivity Center

    - Click on the **Spokes** tab
    - Verify that you see the peer spoke you created previously in **Inactive, pending review** state
    ![hub inactive peer spoke](hub_peer_spoke_inact.png)

1. Accept Peer Spoke
    - Click on the peer-spokes in order to see the details screen.
    - At the top of the screen, we will see an option to Accept or Reject.  Click on **Accept**.
    - Repeat for both spokes

    ![peer accept](peer_accept.png)

1. Confirm that the spokes become active.

    ![peer spoke active](peer_spoke_active.png)

### Discussion

In this task, we completed the cross-project VPC peering process. Acting as the administrator of the central networking hub, we switched back to our primary project. Here, we found the two spoke connection proposals from the application project waiting for approval.

By clicking **Accept** for each proposal, we formally authorized the connection. This action finalized the peering, securely linking the two application VPCs to our transit hub. As a result, all four of our spokes—the two FortiGate NVAs and the two application VPCs—are now in an **Active** state, and GCP can begin exchanging routes between them.

### Proceed to the next section
