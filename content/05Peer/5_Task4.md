---
title: "Task 4 - Verify Firewall"
linkTitle: "Verify Firewall"
chapter: false
weight: 4
---

|                            |    |  
|----------------------------| ----
| **Goal**                   | Ensure proper traffic flow via FortiGate for North/South and East/West traffic from Application servers
| **Task**                   | SSH to applications and run ping tests between local and remote servers.  Configure FortiGate policies to allow the traffic
| **Verify task completion** | When complete, you should be able to ping between the application servers in different VPCs as well as ping the Remote Server from Application servers

{{% notice info %}} Prior to starting this section, log into FortiGate 1 and ensure that you turned the remote IPsec tunnel back up.{{% /notice %}}

## Verify traffic

1. The Star topology in GCP NCC ensures that **edge** spokes can't talk to one another directly.  We set up the FortiGates as Center spokes, the application spokes must now traverse the Fortigate for inter-vpc communication or North/South connectivity to remote sites.

    - Navigate back to the application (peered) project
    - Using the Hambuger menu on the top left of the screen, navigate to Compute Engine > VM instances
    - Open the details for each VM and click on SSH to open SSH-in-browser sessions for both.
    - Start a ping from server 1 to server 2 ``` ping 10.20.0.2 ```  This should fail.

1. Create Address objects for the subnets containing the two servers.
    - Navigate to Fortigate GUI ``` https://<fortigate1 public ip>:8443 ``` in your browser
    - Navigate to Policy & Objects > Addresses
    - Click **Create** and add an address for each Central CIDR

    ![app1 cidr](app1_cent_cidr.png)

    ![app2 cidr](app2_cent_cidr.png)

1. Create a policy allowing the traffic

    - Navigate to Policy & Objects > Firewall Policy 
    - Click **Create new**
    - Configure the policy as below.  Anything not visible is left as default value

    ![East West Pol](e_w_pol.png)

1. Verify that ping is working from 1 to server 2

1. Attempt connectivity to remote site from server 1

    - Start a ping from server 1 to server 2 ``` ping 192.168.100.2 ```  This should fail.

1. Create a policy allowing the traffic on FortiGate1

    - Navigate to Policy & Objects > Firewall Policy 
    - Click **Create new**
    - Configure the policy as below.  Anything not visible is left as default value

    ![fgt remote](fgt1_remote.png)

1. Create a policy allowing the traffic on Remote Fortigate

    - Navigate to Policy & Objects > Firewall Policy 
    - Click **Create new**
    - Configure the policy as below.  Anything not visible is left as default value

    ![fgt1 in](rmt_fgt1_in.png)

1. Verify connectivity to remote site from server 1

    - Start a ping from server 1 to server 2 ``` ping 192.168.100.2 ``` This should now succeed.

### Discussion

In this final hands-on task, we implemented the security policies that govern traffic flow. We started by demonstrating that, by design, all traffic between our application VPCs (East-West) and from our VPCs to the remote network (North-South) was blocked by the FortiGate. We then logged into the FortiGate appliances and created specific, granular firewall policies to explicitly permit this traffic. By successfully pinging between all locations after creating the policies, we have verified that our FortiGates are now acting as the central enforcement point for all traffic, securing our entire hybrid cloud network.

### Chapter Summary

This chapter was dedicated to integrating our separate application VPCs into the secure transit hub we built.

*   **Task 1: Configure Application VPC Spokes:** We initiated the connection by creating VPC spoke proposals from within the application project, pointing them towards our central NCC hub.
*   **Task 2: Accept Application Spokes:** Acting as the network administrator, we switched back to the networking project and accepted the incoming spoke requests, officially activating the peering between the application VPCs and the hub.
*   **Task 3: Verify Routing:** We manipulated the VPC routing tables, removing the direct internet gateways and confirming that the application VPCs learned the new default route from the FortiGates, forcing all traffic through them for inspection.
*   **Task 4: Verify Firewall:** Finally, we configured the firewall policies on our FortiGates to allow the desired East-West and North-South traffic flows, completing the setup of our secure, centralized transit architecture.


## Congratulations!  You have completed the lab work!  Now you can click on the below link to participate in a short Capture the Flag style game.

**You will be prompted for a username. Pick something fun!**

<div style="text-align: center; padding: 40px; border: 1px solid #ccc; border-radius: 8px; background-color: #f9f9f9;">
  <h4 style="color: #333; margin-bottom: 20px;">🎮 Interactive CTF Challenge</h4>
  <p style="color: #666; margin-bottom: 30px;">
    Launch the Capture The Flag challenge in a new window to test your skills!
  </p>
  <button onclick="window.open('https://ctf-cookies-o32xpkqaca-uc.a.run.app/', '_blank', 'width=1200,height=800,scrollbars=yes,resizable=yes')" 
          style="padding: 15px 30px; font-size: 18px; font-weight: bold; color: white; background-color: #007cba; border: none; border-radius: 8px; cursor: pointer; box-shadow: 0 4px 8px rgba(0,0,0,0.1); transition: all 0.3s ease;">
    🚀 Launch CTF Challenge
  </button>
  <br><br>
  <small style="color: #888;">
    Opens in a new window for the best experience
  </small>
</div>

<style>
  button:hover {
    background-color: #0056b3 !important;
    transform: translateY(-2px);
    box-shadow: 0 6px 12px rgba(0,0,0,0.15) !important;
  }
</style>