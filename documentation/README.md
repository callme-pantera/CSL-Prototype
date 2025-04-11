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

I proceeded with the installation and setup of Proxmox on my Acer laptop but encountered several issues. When I attempted to apply the configurations I had made, an error kept occurring despite my various attempts to fix it. The root of the problem was that the Proxmox installer had difficulties with the laptop's partition. I tried multiple solutions, but unfortunately, none were successful.

To keep a long story short, the real reason Proxmox had issues with my laptop’s partition was that the eMMC storage was damaged or faulty. That said, even if it hadn’t been damaged, it still wouldn’t have worked—because **Proxmox doesn’t support eMMC storage.**

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

**IPMI**, short for **I**ntelligent **P**latform **M**anagement **I**nterface, is a standardized interface used for out-of-band management of servers. It allows administrators to monitor, manage, and troubleshoot a system independently of the operating system, and even when the server is powered off, as long as it's connected to power and the network.

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

Before actually starting the OPNsense firewall VM, I needed to ensure that vmbr2 could access the internet through a NAT (Network Address Translation) mechanism. To achieve this, I created a custom shell script that configures Proxmox to forward traffic from the vmbr2 subnet (10.10.0.0/24) to the main WAN interface (vmbr0), allowing outbound internet access for the OPNsense firewall.<br>

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

### OPNsense VM installation and configuration

After the successful **NAT** configuration for *vmbr2*, I started the VM and proceeded with the OPNsense installation. Unfortunately, after starting the VM, it crashed with an error stating that the *vmbr2* bridge does not exist. I verified this, and indeed, the bridge was missing because the section for *vmbr2* was completely absent from the `/etc/network/interfaces` file. I had to manually add the configuration and reload the network. If you want to know how I did it, you can read through the troubleshooting process [here]() or in the troubleshooting folder at the top.

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

### OPNsense web GUI configuration
I decided to go through the "Wizard" configuration option because it includes all the basic settings in one place. This saved me time, as I didn’t have to click through the entire menu to reach each configuration section. I won’t document it very thoroughly since it’s pretty self-explanatory, but here are some of the configurations I made:<br>

<div>
  <img src="/assets/images/OPNsense-wizard1.png" style="width: 100%;">
  <img src="/assets/images/OPNsense-wizard2.png" style="width: 100%;">
  <img src="/assets/images/OPNsense-wizard3.1.png.png" style="width: 100%;">
  <img src="/assets/images/OPNsense-wizard3.2.png" style="width: 100%;">
  <img src="/assets/images/OPNsense-wizard3.3.png.png" style="width: 100%;">
  <img src="/assets/images/OPNsense-wizard4.png" style="width: 100%;">
  <img src="/assets/images/OPNsense-wizard5.png" style="width: 100%;">
</div>



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

