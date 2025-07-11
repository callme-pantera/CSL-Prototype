# VPN Troubleshooting Documentation (WireGuard / OPNsense / Windows Client)

## Introduction
This document summarizes the full troubleshooting process of attempting to integrate WireGuard VPN access to the **CSL** from an external Windows client, through OPNsense firewall with VLANs and routed internal lab networks.

The goal was to:

- Establish a stable VPN tunnel to OPNsense (VPN Gateway)
- Route internal lab VLAN networks via the VPN
- Maintain local internet and Proxmox access on the client

The result was ultimately a design change due to consistent Windows client limitations.<br>

## VPN Lab Design (Initial Plan)

```
[ Windows Laptop ] -- VPN Tunnel (WireGuard) --> [ OPNsense Firewall ] --> [ CSL VLANs / Proxmox ]
```

| Network      | Purpose                      |
| ------------ | ---------------------------- |
| VPN subnet   | VPN address pool             |
| Admin VLAN   | Admin VLAN network (Kali VM) |
| Home network | Home LAN (Proxmox, LAN)      |


## Summary of Problems and Attempts

### 1. VPN Tunnel Establishment
**Successful**

* VPN tunnel from Windows client to OPNsense established without issues.
* Client was reachable via VPN IP, OPNsense VPN interface responded.

### 2. OPNsense to VLAN Reachability
**Successful**

* From OPNsense, ping to internal VM in Admin VLAN worked.
* Proxmox VLAN aware bridges were properly configured.

### 3. Windows Client to VLAN Traffic Failure
**Main problem**

* Ping and any traffic from Windows client to internal VLAN subnets failed consistently.

#### Symptoms:

* Request timed out (ping Admin VLAN target)
* OPNsense rules were fully open (VPN net --> any, VLAN --> any)
* Proxmox VLAN tagging correctly configured
* VPN routing from OPNsense to VLAN functional

<br>

## Full Troubleshooting Attempts

### Tunnel Settings Checked

* AllowedIPs: Tested with VPN subnet only, VPN + VLAN subnet, and full tunnel mode
* PersistentKeepalive enabled
* WireGuard handshake always successful

### Routing Table Checks

* Verified via `route print` on Windows
* Problematic static route VLAN subnet → on-link → VPN client IP kept appearing
* This "on-link" route caused Windows to assume the VLAN network existed locally → packets never entered VPN tunnel → blackhole.

### Disable reply-to in OPNsense

* OPNsense 25.1: reply-to set to `none` under firewall rules
* Implemented on VPN interface to avoid asymmetric routing
* No effect on Windows routing behavior

### Testing with Forced Source Ping

```
ping -S VPN_Client_IP VLAN_Target_IP                    # --> failed
```

### Windows Firewall & Antivirus checked

* Disabled for test --> no difference

### Proxmox VLAN configuration verified

* All vmbr bridges: VLAN aware enabled
* VM: VLAN tag set on VM interface

### Testing with Full Tunnel mode

* Even full tunnel mode did not solve it (due to Windows on-link route error)

### Windows elevated commands attempted

```
route delete VLAN_Subnet
route delete VLAN_Broadcast
```

* Required elevation (admin prompt)
* Routes disappeared after tunnel restart
* Problem persisted after tunnel re-establish

<br>

## Root Cause (Confirmed)

The Windows WireGuard client suffers from a well-documented but unresolved design flaw:

> *If a subnet is added to AllowedIPs, Windows creates a local "on-link" route and assumes the subnet exists on the WireGuard interface. Since the interface has no IP in that subnet, any traffic to that network is silently dropped (blackhole).*

This behavior does not occur on Linux, macOS, iOS, or Android clients.

<br>

## Final Decision and Lab Design Change
After days of structured testing and validation:

* All other factors (firewall, routing, Proxmox, VLANs) were confirmed functional.
* The root cause was purely the Windows client's handling of routing tables.

### Decision:

> Abandon WireGuard VPN approach for CSL Lab access from Windows.
> Implement an alternative internal VPN to the home network to allow direct access to CSL infrastructure.

### Reason:

* This eliminates the Windows on-link route bug entirely.
* Allows reliable access to all lab VLANs and devices via local LAN routing once VPN is active.
* Simplifies administration and removes the need for complex VPN + split routing setup.

<br>

## Conclusion
This troubleshooting process showed:

* Strong understanding and implementation of VPN, VLAN, routing and firewall principles.
* Full isolation of the issue to a Windows client-specific limitation.
* The choice to pivot to an alternate VPN design demonstrates practical real-world IT decision making, prioritizing reliability over theoretical design purity.

The **CSL** now runs without dependency on WireGuard for Windows, and all lab access functions reliably via the internal VPN tunnel.
