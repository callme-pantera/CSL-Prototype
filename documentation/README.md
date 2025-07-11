<div align="center">
  <div style="text-align: center;">
    <picture>
      <source media="(prefers-color-scheme: dark)" srcset="/assets/images/stock/pantera-1.4.png">
      <source media="(prefers-color-scheme: light)" srcset="/assets/images/stock/pantera-1.3.png">
      <img src="/assets/images/stock/pantera-1.4.png" alt="Logo of Pantera" width="350px">
    </picture>
  </div>

  <br>

  [![Issues Badge](https://img.shields.io/badge/ISSUES-0-Test?style=for-the-badge&logo=https%3A%2F%2Ficons8.com%2Ficon%2F83178%2Fimage-file&labelColor=%23333333&color=%23ba181b)](https://github.com/callme-pantera/CSL-prototype/issues)
  [![Stars Badge](https://img.shields.io/badge/STARS-1-Test?style=for-the-badge&logo=https%3A%2F%2Ficons8.com%2Ficon%2F83178%2Fimage-file&labelColor=%23333333&color=%23f6aa1c)](https://github.com/callme-pantera/CSL-prototype/stargazers)
  [![Commits Badge](https://img.shields.io/github/commit-activity/m/callme-pantera/CSL-prototype?style=for-the-badge&label=COMMITS&logo=https%3A%2F%2Ficons8.com%2Ficon%2F83178%2Fimage-file&labelColor=%23333333&color=%237678ED)](https://github.com/callme-pantera/CSL-prototype/commits/main/)
  [![License Badge](https://img.shields.io/badge/LICENSE-CC-Test?style=for-the-badge&logo=https%3A%2F%2Ficons8.com%2Ficon%2F83178%2Fimage-file&labelColor=%23333333&color=%234361ee)](LICENSE)
</div>

<br>

# Installation and Configuration
Before I actually began the project, I wanted to install and configure all the necessary software, ISO files, etc., so that everything was ready for the CSL. The goal was to prepare everything in advance and save time during the different stages of the project.

<br>

## Proxmox
For the Proxmox installation and configuration, I used the [documentation](https://www.proxmox.com/en/proxmox-virtual-environment/get-started) provided by Proxmox themselves and followed it step by step. Keep in mind to read carefully and not just click through—take the time to understand each step.

<br>

> [!CAUTION]
> Installing the Proxmox ISO Installer will permanently overwrite the disk it is installed on, as it is a bare-metal installer. This means that any existing data will be permanently removed. Proceed with caution!

<br>

You should be able to install it on your own, as the documentation is quite clear. Here are the summed up steps I followed for the installation:

- Download the Proxmox ISO image installer (*in my case the 8.2-2 version*)
- Download [Rufus](https://rufus.ie/de/) or another USB burning tool
- Burn the Proxmox ISO onto a USB drive with at least 8GB of free space
- I used my old laptop to boot Proxmox from the USB drive --> BIOS
- Proceed with the Proxmox installation

<br>

> [!WARNING]  
> As mentioned earlier, burning this disk with the Proxmox ISO file will permanently erase **ALL DATA** on the selected disk!

<br>

<h3>Rufus Setup</h3>
<div style="display: flex; justify-content: space-between;">
  <img src="/assets/images/rufus-1.png" alt="Rufus 1" style="width: 45%;" />
  <img src="/assets/images/rufus-2.png" alt="Rufus 2" style="width: 45%;" />
</div>

<br>

<h3>Proxmox Booting via USB Drive</h3>
<div style="display: flex; justify-content: space-between;">
  <img src="/assets/images/boot-proxmox1.jpg" alt="BIOS boot proxmox" style="width: 45%;" />
  <img src="/assets/images/boot-proxmox22.jpg" alt="BIOS boot proxmox success" style="width: 45%;" />
</div>

<br>

I proceeded with the installation and setup of Proxmox on my Acer laptop but encountered several issues. When I attempted to apply the configurations I had made, an error kept occurring despite my various attempts to fix it. The root of the problem was that the Proxmox installer had difficulties with the laptop's partition. I tried multiple solutions, but unfortunately, none were successful.<br>
To keep a long story short, the real reason Proxmox had issues with my laptop’s partition was that the eMMC storage was damaged or faulty. That said, even if it hadn’t been damaged, it still wouldn’t have worked—because **Proxmox doesn’t support eMMC storage.**<br>

If you’d like to read through my troubleshooting process, you can find all the documentation in the ***troubleshooting folder*** or [here](/troubleshooting/TS-proxmox-laptop-issue.md).

<br>

> [!TIP]  
> My ACER Switch Alpha 12 came with eMMC storage, a budget-friendly option found in low-cost devices. While sufficient for basic tasks, eMMC lacks the speed and durability of SSDs, which are better suited for long-term, intensive use.

<br>

Since my laptop wasn’t suitable for hosting Proxmox, I had to improvise. Luckily, I had an incredible teacher who gifted me an old Supermicro 'server' with 2 TB of storage. Long story short: I offered to buy it from him, but he refused—he insisted on giving it to me for free.

<br>

<div style="display: flex; justify-content;">
  <img src="/assets/images/boot-order-prxmx.jpg" alt="Boot Order" style="width: 100%;" />
</div>

<br>

To enter the BIOS, press either *Del* or *F2* repeatedly during startup. Once inside, navigate to the ***Boot*** section and, as shown in the screenshot, change **Boot Option 1** to **UEFI USB Key:UEFI**. If you don’t need to configure anything else, you can press *F4* to save and exit, or go to the ***Save & Exit*** tab and reboot the server.<br>
If your system allows you to configure **IPMI**, I sincerely recommend doing so—it’s an incredibly useful and powerful feature to have.

<br>

### IPMI Configuration
After configuring the BIOS boot order on the server, I also wanted to check whether IPMI was enabled and functioning—and in my case, it was. However, I had to configure the station IP address, subnet mask, gateway, etc., since it wasn’t adapted to my router / ISP settings.

<br>

> [!IMPORTANT]  
> Make sure to ***check your ISP’s DHCP range*** so that you don’t assign an IP address within that scope, which could potentially cause conflicts!

<br>

**IPMI**, short for **I**ntelligent **P**latform **M**anagement **I**nterface, is a standardized interface used for out-of-band management of servers. It allows administrators to monitor, manage, and troubleshoot a system independently of the operating system, and even when the server is powered off, as long as it's connected to power and the network.<br>

When **IPMI** is enabled, I can access the server remotely through a dedicated management interface—often via a web-based dashboard or console—regardless of the server’s current state. This makes it possible to perform tasks like system reboots, BIOS configuration, or OS installations without needing physical access to the machine. It’s an incredibly useful and powerful feature, especially for server maintenance and remote administration!<br>
Most of the time, the IPMI LAN port is located separately from the other LAN ports. In my case, the IPMI port is isolated from the four regular LAN ports and positioned on the far left.

<br>

### Proxmox and IPMI Dashboards
Again after the reboot, leave the USB drive plugged in and wait for the Proxmox installation process to start. Once it appears, choose the **Graphical Install**, as it is more user-friendly.

- Select the target hard disk: `/dev/sda`  
- Enter your **location** and **time zone**  
- Create a **strong admin password**  
- Use a **valid email address**, which will be used for important alerts and notifications from the server  
- Choose a **fitting hostname**  
- Configure the **IP address (CIDR)**, **gateway**, and **DNS** (`1.1.1.1` for Cloudflare or `8.8.8.8` for Google)

Once you’ve completed the configuration and the installation finishes, **reboot your server/laptop/machine** (whatever you're using), and **unplug the USB stick** so the installed Proxmox system can load.<br>
After a while, you’ll be prompted to log in. The default login is root, using the password you defined during the Proxmox installation.

<br>

Here's what the IPMI interface looks like once you're logged in, and also how the Proxmox web GUI should appear after successful login:

<div>
  <img src="/assets/images/IPMI-preview.png" alt="IPMI" style="width: 100%;" />
  <img src="/assets/images/Proxmox-preview.png" alt="Proxmox web GUI" style="width: 100%;" />
</div>

<br>

## OPNsense Firewall
Once I logged into my Proxmox server, I began installing and configuring the OPNsense firewall. As mentioned earlier, I had already downloaded all necessary resources (ISO files etc.) in advance to save time throughout the project.<br>

To properly integrate the firewall into my virtual lab environment, I first created two additional Linux bridges:

  - ***vmbr1*** serves as the *internal LAN bridge*. It functions as a virtual switch, connecting all internal lab VMs and VLANs to the OPNsense instance. This bridge is dedicated to traffic inside the lab, enabling segmentation and routing through the firewall.

  - ***vmbr2*** acts as the *virtual WAN interface*. It provides the OPNsense firewall with access to the internet through a **NAT** (Network Address Translation) configuration. This setup allows outbound connectivity without exposing the internal lab network or interfering with the physical home network.

I intentionally **avoided using vmbr0** (the default Proxmox WAN bridge) for the OPNsense WAN interface. *vmbr0* is directly connected to my home router and is used by the Proxmox host itself. Using *vmbr0* inside the firewall VM would have created a conflict, as the OPNsense VM, my home router, and the Proxmox host would all attempt to use the same default gateway, leading to **routing issues and potential network disruptions**.<br>

By isolating the *WAN traffic* of the OPNsense VM on *vmbr2* and implementing *NAT* on the Proxmox host, I ensured that the firewall has internet access without overlapping with or disrupting the production network. This approach also **provides a safe and controlled boundary** between the lab environment and the external network, which is essential for cybersecurity.

<br>

> [!TIP]
> When creating the firewall VM, make sure to check the 'VLAN Aware' box to ensure communication with other VLANs.

<br>

<div>
  <img src="/assets/images/prxmx-fw-config1.png" style="width: 100%;">
</div>

<br>

> [!NOTE]
> Notice that I’ve selected *vmbr1* (the LAB LAN) instead of *vmbr0* (the WAN). Once I’ve finished configuring the firewall, I’ll return to this point and update it accordingly to include *vmbr2*.

<br>

<div>
  <img src="/assets/images/prxmx-fw-config2.png" style="width: 100%;">
  <img src="/assets/images/prxmx-fw-config3.png" style="width: 100%;">
</div>

<br>

Before actually starting the OPNsense firewall VM, I needed to ensure that *vmbr2* could access the internet through a NAT (Network Address Translation) mechanism. To achieve this, I created a custom shell script that configures Proxmox to forward traffic from the *vmbr2* subnet to the main WAN interface (*vmbr0*), allowing outbound internet access for the OPNsense firewall.<br>

This approach provides full internet connectivity for the virtual WAN interface without interfering with the physical home network or reusing the host’s default gateway. In addition, I created a second shell script that cleanly removes the NAT configuration in case I need to disable or undo these changes in the future.<br>

Below are both scripts:

<br>

> [!TIP]
> Before running the scripts, you can create manual backups of all affected system files. This ensures you can easily restore the previous state if needed. 
> 
> Use the following commands on your Proxmox host:
>
> ```
> $ cp /etc/network/interfaces /etc/network/interfaces.bak
> $ cp /etc/sysctl.conf /etc/sysctl.conf.bak
> $ iptables-save > /root/iptables-before.txt
> ```

<br>

<div>
  <img src="/assets/images/backup_interfaces+sysctl.conf.png" style="width: 100%;">
</div>

<br>

<h3>NAT Shell-Script</h3>

```
#!/bin/bash
# NAT configuration script for vmbr2 → vmbr0
# Author: Pantera

# Enable IPv4 forwarding
echo 1 > /proc/sys/net/ipv4/ip_forward
sed -i 's/^#net.ipv4.ip_forward=.*/net.ipv4.ip_forward=1/' /etc/sysctl.conf
sysctl -w net.ipv4.ip_forward=1

# Set up NAT rule
iptables -t nat -A POSTROUTING -s x.x.x.x/24 -o vmbr0 -j MASQUERADE

# Save rules
iptables-save > /etc/iptables.up.rules

# Create persistent loader
cat <<EOF > /etc/rc.local
#!/bin/sh -e
iptables-restore < /etc/iptables.up.rules
exit 0
EOF

# Make /etc/rc.local executable
chmod +x /etc/rc.local
```

```
$ chmod +x /xyz/nat-setup.sh
```

<br>

<h3>NAT Uninstall Shell-Script</h3>

```
#!/bin/bash
# NAT configuration script for vmbr2 → vmbr0
# Author: Pantera
# Uninstall NAT config

# Remove NAT rule
while iptables -t nat -C POSTROUTING -s x.x.x.x/24 -o vmbr0 -j MASQUERADE 2>/dev/null; do
    iptables -t nat -D POSTROUTING -s x.x.x.x/24 -o vmbr0 -j MASQUERADE
done

# Remove iptables file
rm -f /etc/iptables.up.rules

# Remove rc.local if created by us
rm -f /etc/rc.local
```

```
$ chmod +x /xyz/nat-uninstall.sh
```

<br>

<div>
  <img src="/assets/images/NAT-script-success-2.png" style="width: 100%;">
</div>

<br>

### Firewall VM Installation and Configuration
After the successful **NAT** configuration for *vmbr2*, I started the VM and proceeded with the OPNsense installation. Unfortunately, after starting the VM, it crashed with an error stating that the *vmbr2* bridge does not exist. I verified this, and indeed, the bridge was missing because the section for *vmbr2* was completely absent from the `/etc/network/interfaces` file. I had to manually add the configuration and reload the network. If you want to know how I did it, you can read through the troubleshooting process [here](/troubleshooting/TS-vmbr2-missing.md) or in the troubleshooting folder at the top.

<br>

Once the VM starts, you’ll see the live-mode login screen, which tells you to log in as either `root` or `installer`. Since this is our first time starting the VM and we need to install the system, we’ll log in as `installer` of course. The default password for both `installer` and `root` is `opnsense`.<br>

- Choose your **keyboard layout**  
- Choose your **installation type** (*I chose ZFS for a modern installation*)  
- Select the **virtual device type** (*I chose Stripe*)  
- Select your **target disk** for the installation  

<br>

> [!NOTE]
> ZFS:
> The recommended 3 GB of RAM for ZFS wasn't a concern in this case, since I can always allocate more RAM via Proxmox if needed. That’s why I went ahead with ZFS.
> 
> Virtual Device Type:
> I chose the stripe option (no redundancy) because this OPNsense instance runs inside Proxmox, where backups are handled separately. Redundancy wasn't necessary for this lab setup.

<br>

<div>
  <img src="/assets/images/OPNsense-login1.png" style="width: 100%;">
  <img src="/assets/images/OPNsenes-download-progress.png" style="width: 100%;">
</div>

<br>

After the successful installation and configuration you should be able to login as root with the standard password or if you've chagned it with yours. Once logged in you'll see 13 Options to choose from. Firstly we're gonna set the interface ip adresses:<br>

- Selected option `2` to configure interface settings  
- Chose the **WAN interface** (`vtnet1`) for configuration  
- Declined the **DHCP option**, as I wanted to assign a static IP address that fits the predefined LAB infrastructure (`vmbr2` - Lab LAN)
- Declined to use the gateway IP as the DNS server, since there is **no DNS service running** at that address  
- Manually set **`1.1.1.1`** as the WAN DNS server (Cloudflare)  
- Skipped IPv6 configuration for now  
- Kept the Web GUI protocol as **HTTPS** (did not switch to HTTP)  
- Chose to generate a **new self-signed Web GUI certificate** for secure access

<br>

- After that again select the option `2` to configure the interface settings
- Choose the **LAN interface** (`vtnet0`) for configuration
- Declined the **DHCP option**, as I wanted to assign a static IP address that fits the predefined LAB infrastructure (`vmbr1` - virtual WAN / NAT)
- Make sure to NOT enter a upstream gateway for this interface
- Skipped IPv6 configuration for now
- Enabled DHCP on LAN 
- Keep the Web GUI protocol as **HTTPS** (did not switch to HTTP) 
- Chose to generate a **new self-signed Web GUI certificate** for secure access

<br>

> [!IMPORTANT]
> To be honest, I don't know how or when it happened to me, but **MAKE SURE** to check whether the Linux bridges (*vmbr*) are correctly assigned to the firewall's network interfaces.
> You'll save yourself hours of troubleshooting by simply verifying that `net0 = vmbr1` (Lab LAN) and `net1 = vmbr2` (virtual WAN NAT).

<br>

Once you've finished the installation and configuration, create a VM with an operating system of your choice and make sure **NOT to attach it to a VLAN** just yet!  
In my case, that would be VLAN 10 for testing — but I need to do that **AFTER** logging into the OPNsense web GUI so I can configure the VLANs there first.  
If you attach the VLAN too early, it may lead to issues with DHCP and internet access for all VMs using VLAN tags.<br>
Once everything is set up in OPNsense, you’ll find the option to assign the VLAN in the VM’s network tab. This configuration step is essential for VLANs to work correctly in OPNsense, since VLANs will be **configured, assigned, and managed directly through the firewall.**
<br>  
This VM will be used for testing purposes, so the configuration doesn't matter too much. In my case, I created a Ubuntu Desktop VM. Here are the configurations I used:

<br>

<div>
  <img src="/assets/images/ubuntu-desktop-preview-vm.png" style="width: 100%;">
</div>

<br>

### OPNsense web GUI Configuration
I decided to go through the "Wizard" configuration option because it includes all the basic settings in one place. This saved me time, as I didn’t have to click through the entire menu to reach each configuration section. I won’t document it very thoroughly since it’s pretty self-explanatory, but here are some of the configurations I made:<br>

<div>
  <img src="/assets/images/OPNsense-wizard1.png" style="width: 100%;">
  <img src="/assets/images/OPNsense-wizard2.png" style="width: 100%;">
  <img src="/assets/images/OPNsense-wizard3.1.png" style="width: 100%;">
  <img src="/assets/images/OPNsense-wizard3.2.png" style="width: 100%;">
  <img src="/assets/images/OPNsense-wizard3.3.png" style="width: 100%;">
  <img src="/assets/images/OPNsense-wizard4.png" style="width: 100%;">
  <img src="/assets/images/OPNsense-wizard5.png" style="width: 100%;">
</div>

<br>

### VLAN10 Configuration
Now that the "Wizard" configuration is complete, I moved on to configure all the VLANs along with the corresponding firewall rules. Here are the summarized steps of how I did it:<br>

#### VLAN Creation

<div>
  <img src="/assets/images/vlan10-creation.png" style="width: 100%;">
</div>

<br>

- Head to `Interfaces > Devices > VLAN`  
- Leave the device name empty; OPNsense will generate a fitting name automatically  
- Choose the correct parent interface (*in my case, `vtnet0` → LAN*)  
- Enter the appropriate VLAN tag (*e.g., 10 for VLAN 10*)  
- Write a clear and descriptive name  
- Save and apply the changes  

<br>

#### Assign New Interface

<div>
  <img src="/assets/images/vlan10-creation2.png" style="width: 100%;">
  <img src="/assets/images/vlan10-creation3.png" style="width: 100%;">
</div>

<br>

- Head to `Interfaces > Assignments`  
- Add the newly created VLAN device and save it  
- Go to `Interfaces > VLAN10Testing` and enable the interface  
- Once enabled, change the **IPv4 Configuration Type** to **Static** and assign a suitable IP address  
- *I also updated the description for a more fitting alternative*

<br>

#### Activate DHCP for VLAN

<div>
  <img src="/assets/images/vlan10-creation4.png" style="width: 100%;">
  <img src="/assets/images/vlan10-creation4.2.png" style="width: 100%;">
</div>

<br>

- Head to `Services > ISC DHCPv4 > LAN_VLAN10` and enable DHCP server on the interface (LAN_VLAN10).
- Enter your IP range and don’t forget to specify a DNS server before saving. (*I chose 1.1.1.1 and 8.8.8.8 — Cloudflare and Google*)

<br>

#### Firewall Rule – Block VLAN10 Access to OPNsense

<div>
  <img src="/assets/images/vlan10-creation5.1.png" style="width: 100%;">
  <img src="/assets/images/vlan10-creation5.2.png" style="width: 100%;">
</div>

<br>

- Head to `Firewall > Rules > LAN_VLAN10` and add a new rule.
- Make sure to block access from VLAN10 to the OPNsense interface, as shown in the screenshots. Since this is a testing VLAN in my case, allowing it to access the firewall while intentionally testing potentially malicious software would pose an **unnecessary security risk**.

<br>

#### Firewall Rule – Allow Internet Access from VLAN10

<div>
  <img src="/assets/images/vlan10-creation6.1.png" style="width: 100%;">
  <img src="/assets/images/vlan10-creation6.2.png" style="width: 100%;">
</div>

<br>

- Head to `Firewall > Rules > LAN_VLAN10` and add a new rule.
- Make sure to allow internet access from VLAN10, as shown in the screenshots above.

<br>

#### NAT Configuration for VLAN10

<div>
  <img src="/assets/images/vlan10-creation7.png" style="width: 100%;">
  <img src="/assets/images/vlan10-creation9.1.png" style="width: 100%;">
  <img src="/assets/images/vlan10-creation9.2.png" style="width: 100%;">
</div>

<br>

- Head to `Firewall > NAT > Outbound` and set the NAT mode to **Hybrid Outbound NAT rule generation**.  
- Create a new rule and make sure to fill it out exactly as shown in the screenshots above.

### VLAN Setup Test
To verify that our VLAN configurations were successful, I ran a series of commands and performed several checks to ensure everything was working as intended. These tests helped confirm that the firewall rules, IP assignments, and network segmentation were all functioning properly.<br>

```
$ ipconfig
```
```
$ ping 8.8.8.8
$ ping x.x.x.x
```
```
$ nslookup opnsense.lab.local
```

<br>

<div>
  <img src="/assets/images/opnsense-test.png" style="width: 100%";>
  <img src="/assets/images/opnsense-test2.png" style="width: 100%";>
  <img src="/assets/images/opnsense-test2.2.png" style="width: 100%";>
  <img src="/assets/images/opnsense-test3.png" style="width: 100%";>
  <img src="/assets/images/opnsense-test4.png" style="width: 100%";>
</div>

<br>

Here is the breakdown of every test I have performed to verify the configuration's validity:<br>

- `ipconfig` confirms that the client **successfully received an IP address** from the OPNsense DHCP server.
- `ping 8.8.8.8` verifies **internet connectivity via ICMP**; packets are reaching Google’s DNS server with 0% loss.
- `ping x.x.x.x` returns *“Destination host unreachable”*, which is **expected** – access to the firewall gateway is intentionally blocked for this VLAN.
- `nslookup opnsense.lab.local` shows that **external DNS is working**, but internal names like `lab.local` are not resolved – **as configured** (no internal DNS or override).
- The **Koenigsegg Agera RS** — a breathtaking marvel of engineering — appearing in a Bing image search: confirmation that DNS resolution and web browsing are **functioning flawlessly** 😂.
- Attempting to access the OPNsense Web GUI **fails as expected** – access from VLAN10 is restricted by firewall rules to prevent potential compromise.

<br>

### Summarized VLAN Configuration for All Remaining VLANs
As shown in the title, I won’t be documenting this step thoroughly, since the principle is the same as with the testing VLAN. Instead, I’ll outline the key steps I took for each VLAN and briefly explain its purpose.<br>

1. **VLAN 20 and VLAN 1**<br>
I followed the same principle for the rules of the Container and Hacking VLANs. These VLANs must have access to the internet but are not allowed to access the firewall itself. The reason for this is that it would be an unnecessary security risk to allow them access to the firewall.

2. **VLAN 77**<br>
For the Omni-Administrative VLAN, I allowed internet access, but contrary to the others, I also allowed it to access the firewall. The reason being that since this is an admin VLAN, it should have access for configurations, troubleshooting, updates, monitoring, etc.

<br>

## Container Environment - VLAN20
As the foundational step of this project, I deployed an Ubuntu Server to serve as the base system for my Docker host. I deliberately chose Ubuntu due to its stability, widespread adoption, and extensive documentation—making it an ideal candidate for server-based workloads and long-term maintainability.<br>

For containerization, I opted for Docker. This decision was based on my existing experience and proficiency with Docker’s architecture, CLI tooling, and operational best practices. Leveraging a platform I'm already familiar with allows me to build and iterate on the project efficiently, without the overhead of learning a new container ecosystem from scratch.<br>

During the installation, I manually configured the IP address and network settings. This was done intentionally to ensure a consistent and predictable network environment within my CSL. Static addressing gives me full control over routing, VLAN mapping, and access control—critical for simulating real-world cybersecurity infrastructure.

<br>

<div>
  <img src="/assets/images/edit-ip4-ubunut-server.png" style="width:100%";>
</div>

<br>

After successfully installing and configuring the system, I proceeded with the Docker setup. However, before that, I went through the following chapter. You don’t have to do this, it’s optional, and you can skip it if you prefer to continue directly.

<br>

> [!NOTE]
> The next chapter on VPN configuration is **NOT** necessary. I chose to include it because it made sense to me personally, and I had always wanted to try it out.

<br>

### Remote SSH Access to VLAN-Tagged VMs and Server Resources via WireGuard VPN

The goal is to access a VM running in **VLAN20** via **SSH** from a remote client connected through a **ISP-generated WireGuard VPN**.
The remote client has **no access to the VPN server configuration** and receives a **/32 IP** from my ISP.

<br>

| Component         | Detail                                  |
| ----------------- | --------------------------------------- |
| VPN Client        | Receives IP: `x.x.x.x/32` (no gateway)  |
| Proxmox Host      | Manages networking + NAT                |
| VM                | Ubuntu Server `x.x.x.x/24`, VLAN20      |
| Bridge Used       | `vmbr20` with IP `x.x.x.x`              |
| Interface Tagging | VLAN Tag `20` used on `eno1.20`         |
| Proxmox IP (LAN)  | `x.x.x.x`                               |

<br>

- The WireGuard VPN tunnel is **fully controlled by my ISP**, no changes can be made on the server side.
- The VPN client is isolated (`/32` subnet, no gateway).
- To **bridge this limitation**, Proxmox acts as a **NAT Gateway** and optionally as a **SSH proxy** via **port forwarding**.
- Proxmox interface `vmbr20` handles VLAN20 and must have the **default gateway IP of the VM**.

<br>

> [!NOTE]
> As you’ve probably noticed, I’ve purposefully censored certain technical details. This isn’t a tutorial anyway (*as mentioned on the first page*), so I’m sure you’ll understand.

<br>

#### Configure `vmbr20` on Proxmox (VLAN20)
In Proxmox UI or via `/etc/network/interfaces`:

```bash
auto vmbr20
iface vmbr20 inet static
    address x.x.x.x/24
    bridge_ports eno1.20
    bridge_stp off
    bridge_fd 0
```

Ensure:

* `eno1.20` exists as a VLAN subinterface or use `bridge-vlan-aware` on `vmbr1` as alternative.
* Apply config or reboot host.

#### Ubuntu VM Network (Manual or Netplan)
Inside `x.x.x.x` (Ubuntu Server VM):

```bash
$ ip addr add x.x.x.x/24 dev ens18
$ ip route add default via x.x.x.x
```

Or via Netplan (`/etc/netplan/01-netcfg.yaml`):

```yaml
network:
  version: 2
  ethernets:
    ens18:
      dhcp4: no
      addresses:
        - y.y.y.y/24
      gateway4: y.y.y.y
```

Then apply:

```bash
$ sudo netplan apply
```

#### Enable SSH on the VM
Ensure:

```bash
$ sudo systemctl enable ssh
$ sudo systemctl start ssh
```

Check:

```bash
$ sudo ss -tnlp | grep :22
```

#### Enable NAT on Proxmox Host
This step allows the VPN client to access the VM even though it has no valid return path (`/32` IP, no gateway).

```bash
$ echo 1 > /proc/sys/net/ipv4/ip_forward
$ iptables -t nat -A POSTROUTING -s x.x.x.x -j MASQUERADE
```

> Make it persistent:

```bash
$ apt install iptables-persistent -y
$ netfilter-persistent save
```

#### Optional: Add Route to Handle VPN Client
Not always necessary, but if needed:

```bash
$ ip route add x.x.x.x dev vmbr0
```

#### Optional: Port Forwarding as Fallback
If NAT does not work (e.g. WireGuard or ISP blocks return routes), use Proxmox port forwarding:

```bash
$ iptables -t nat -A PREROUTING -i vmbr0 -p tcp --dport xxxx -j DNAT --to-destination x.x.x.x:22
$ iptables -A FORWARD -p tcp -d x.x.x.x --dport 22 -j ACCEPT
```

Then SSH from VPN client via:

```bash
$ssh -p xxxx user@x.x.x.x
```

#### Test SSH Access from VPN Client
Ensure you're connected to WireGuard. Then:

```bash
$ ssh user@x.x.x.x      # If NAT works
$ ssh -p xxxx user@x.x.x.x  # If using port forwarding
```

<br>

#### Tip: Secure the VM and Proxmox
Once access is stable, go to the main Proxmox shell and type the following commands:<br>

```bash
$ apt update
$ apt install sudo -y
$ adduser newuser
$ usermod -aG sudo newuser
```

After that, you should be able to log in as the new user via SSH and have sudo privileges. Once this is confirmed, disable the option to log in as root via SSH in the Proxmox shell.<br>

```bash
$ sudo nano /etc/ssh/sshd_config
# Set:
PermitRootLogin no
PasswordAuthentication no
$ sudo systemctl restart ssh
```

<br>

### Docker Container Environment Installation
This chapter includes installing a secure, up-to-date version of Docker Engine, Docker CLI, containerd, and the Docker Compose plugin on Ubuntu Server.<br>

#### Install prerequisites and keyring directory

```bash
$ sudo apt install ca-certificates curl gnupg -y
$ sudo install -m 0755 -d /etc/apt/keyrings
```

* `ca-certificates`: Required to validate HTTPS connections securely
* `curl`: Downloads files from HTTPS sources (used later for Docker GPG key)
* `gnupg`: Used to verify package authenticity
* `/etc/apt/keyrings`: A modern and secure place to store third-party GPG keys

<br>

#### Add Docker’s official GPG key

```bash
$ curl -fsSL https://download.docker.com/linux/ubuntu/gpg | \
sudo gpg --dearmor -o /etc/apt/keyrings/docker.gpg
```

* Ensures all Docker packages you install are **signed and verified** by Docker Inc.
* Prevents installation of **tampered or malicious packages**.

<br>

#### Add the Docker APT repository

```bash
echo \
"deb [arch=$(dpkg --print-architecture) signed-by=/etc/apt/keyrings/docker.gpg] \
https://download.docker.com/linux/ubuntu $(lsb_release -cs) stable" | \
sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
```

* Adds the official Docker repository to your system.
* Ensures that Docker-related packages are always pulled from a **trusted and up-to-date source**.

<br>

#### Update APT and install Docker

```bash
$ sudo apt update
$ sudo apt install docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin -y
```

**Why install these components?**

| Package                 | Purpose                                                             |
| ----------------------- | ------------------------------------------------------------------- |
| `docker-ce`             | Docker Engine (daemon + runtime)                                    |
| `docker-ce-cli`         | Command-line interface                                              |
| `containerd.io`         | High-performance container runtime                                  |
| `docker-buildx-plugin`  | Extended build support (multi-arch, caching, etc.)                  |
| `docker-compose-plugin` | Native Compose v2 support (replaces legacy `docker-compose` binary) |

<br>

#### Enable Docker usage without `sudo`

```bash
$ sudo usermod -aG docker $USER
```

* Adds your current user to the `docker` group.
* This allows you to run Docker commands **without needing `sudo`** every time.

> [!NOTE]
> Requires re-login to take effect (see next step).

<br>

#### Apply new group permissions

```bash
$ newgrp docker
```

Or log out and back in again.

* Linux only applies group membership changes at login.
* Without this, you’ll still get permission errors when using `docker`.

<br>

#### Test the installation

```bash
$ docker run hello-world
```

**What it does:**

* Downloads and runs a test container from Docker Hub.
* Confirms that Docker Engine and networking work as expected.

You should see a message like:

<div>
  <img src="/assets/images/docker-success-hello-docker.png" style="width: 100%";>
</div>

<br>

#### Enable Docker at boot

```bash
$ sudo systemctl enable docker
```

Ensures Docker starts automatically with the system.

<br>

```bash
$ docker --version
$ docker compose version
```

Verifies that both the CLI and Compose are installed correctly.

<br>





<br>
<br>
<br>
<br>

### Test 123
> [!NOTE]  
> Highlights information that users should take into account, even when skimming.

> [!TIP]
> Optional information to help a user be more successful.

> [!IMPORTANT]  
> Crucial information necessary for users to succeed.

> [!WARNING]  
> Critical content demanding immediate user attention due to potential risks.

> [!CAUTION]
> Negative potential consequences of an action.

#### Test 321

##### Test 123321

# Sources

- YouTube:  [Gerard O'Brien, Building the Ultimate Cybersecurity Lab](https://www.youtube.com/watch?v=XIvn0ZDSmKA&list=PL3ljjyal211AbTqlxSo6CGBiVqsXw8wrp&index=22)
- Medium:   [TheInfoSec Guy](https://medium.com/@jibingeorge.mg/cybersecurity-research-lab-setup-5beb54d8dd59)
- ChatGPT:  [OpenAI](https://chatgpt.com/)
- Friends

