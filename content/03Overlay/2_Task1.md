---
title: "FortiGate BGP over IPSec"
linkTitle: "FortiGate Overlay"
chapter: false
weight: 2
---

## Configure IPSec Tunnels and BGP on FortiGate

In order to save time during this lab, we will use SSH to access FortiGates and copy/paste in CLI commands

{{% notice warning %}} You will need to use your notepad to replace the remote-gw values in the below CLI Templates with values from your environment! {{% /notice %}}

1. Log into the **remote FortiGate** using ``` ssh admin@<fortigate public ip> ``` password will be ```Fortinet1234$```
      
  - Copy the below configurations and modify the values in "<>" to match your environment

    ```sh
    
    config system settings
        set bfd enable
    end

    config vpn ipsec phase1-interface
        edit FGT-1
            set interface port1
            set ike-version 2
            set peertype any
            set net-device disable
            set proposal aes256-sha384
            set dhgrp 5
            set remote-gw <fgt1-public-ip>
            set psksecret Fortinet1234$
        next
        edit FGT-2
            set interface port1
            set ike-version 2
            set peertype any
            set net-device disable
            set proposal aes256-sha384
            set dhgrp 5
            set remote-gw <fgt2-public-ip>
            set psksecret Fortinet1234$
        next
    end

    config vpn ipsec phase2-interface
        edit FGT-1
            set phase1name FGT-1
            set proposal aes128-sha1 aes256-sha1 aes128-sha256 aes256-sha256 aes128gcm aes256gcm chacha20poly1305
        next
        edit FGT-2
            set phase1name FGT-2
            set proposal aes128-sha1 aes256-sha1 aes128-sha256 aes256-sha256 aes128gcm aes256gcm chacha20poly1305
        next
    end

    config system interface
        edit FGT-1
            set vdom root
            set ip 10.17.1.1 255.255.255.255
            set type tunnel
            set remote-ip 10.17.1.2 255.255.255.255
            set interface port1
        next
        edit FGT-2
            set vdom root
            set ip 10.17.2.1 255.255.255.255
            set type tunnel
            set remote-ip 10.17.2.2 255.255.255.255
            set interface port1
        next
    end

    config firewall policy
        edit 0
            set name FGT1-out
            set srcintf port2
            set dstintf FGT-1
            set action accept
            set srcaddr all
            set dstaddr all
            set schedule always
            set service HTTP
        next
        edit 0
            set name FGT2-out
            set srcintf port2
            set dstintf FGT-2
            set action accept
            set srcaddr all
            set dstaddr all
            set schedule always
            set service HTTP
        next
    end


    config router bgp
        set as 65200
        config neighbor
            edit 10.17.1.2
                set remote-as 65200
                set next-hop-self enable
                set soft-reconfiguration enable
                set bfd enable
            next
            edit 10.17.2.2
                set remote-as 65200
                set next-hop-self enable
                set soft-reconfiguration enable
                set bfd enable
            next
        end
        config network
            edit 1
                set prefix 192.168.100.0 255.255.255.0
            next
        end

    ```

2. Log into **FortiGate 1** using ``` ssh admin@<fortigate public ip> ``` password will be ```Fortinet1234$```

  - Copy the below configurations and modify the values in "<>" to match your environment

  ```sh
  config system settings
        set bfd enable
    end
  config vpn ipsec phase1-interface
      edit RMT-FGT
          set interface port1
          set ike-version 2
          set peertype any
          set net-device disable
          set proposal aes256-sha384
          set dhgrp 5
          set remote-gw <remote-fgt-public-ip>
          set psksecret Fortinet1234$
      next
  end

  config vpn ipsec phase2-interface
      edit RMT-FGT
          set phase1name RMT-FGT
          set proposal aes128-sha1 aes256-sha1 aes128-sha256 aes256-sha256 aes128gcm aes256gcm chacha20poly1305
      next
  end

  config system interface
      edit RMT-FGT
          set vdom root
          set ip 10.17.1.2 255.255.255.255
          set type tunnel
          set remote-ip 10.17.1.1 255.255.255.255
          set interface port1
      next
  end

  config firewall policy
      edit 0
          set name RMT-FGT-out
          set srcintf RMT-FGT
          set dstintf port2
          set action accept
          set srcaddr all
          set dstaddr all
          set schedule always
          set service HTTP
      next
  end

  config router bgp
      config neighbor
          edit 10.17.1.1
              set remote-as 65200
              set next-hop-self enable
              set soft-reconfiguration enable
              set bfd enable
          next
  end

  ```

3. Log into **FortiGate 2** using ``` ssh admin@<fortigate public ip> ``` password will be ```Fortinet1234$```

  - Copy the below configurations and modify the values in "<>" to match your environment

  ```sh

  config system settings
        set bfd enable
    end

  config vpn ipsec phase1-interface
      edit RMT-FGT
          set interface port1
          set ike-version 2
          set peertype any
          set net-device disable
          set proposal aes256-sha384
          set dhgrp 5
          set remote-gw 34.16.55.175
          set psksecret Fortinet1234$
      next
  end

  config vpn ipsec phase2-interface
      edit RMT-FGT
          set phase1name RMT-FGT
          set proposal aes128-sha1 aes256-sha1 aes128-sha256 aes256-sha256 aes128gcm aes256gcm chacha20poly1305
      next
  end

  config system interface
      edit RMT-FGT
          set vdom root
          set ip 10.17.2.2 255.255.255.255
          set type tunnel
          set remote-ip 10.17.2.1 255.255.255.255
          set interface port1
      next
  end

  config firewall policy
      edit 0
          set name RMT-FGT-out
          set srcintf RMT-FGT
          set dstintf port2
          set action accept
          set srcaddr all
          set dstaddr all
          set schedule always
          set service HTTP
      next
  end

  config router bgp
      config neighbor
          edit 10.17.2.1
              set remote-as 65200
              set next-hop-self enable
              set soft-reconfiguration enable
              set bfd enable
          next
  end

  ```

### Proceed to the next section