# VLAN77 - Tuning of the Kali Linux Omni-VM

In this documentation, I will document the process of advanced enhancing and hardening of the Kali Linux Omni-VM. This is not a tutorial, as mentioned before, but rather a reflection of my thought process and results.

<br>

## Root SSH Login Deactivation
The reason I decided to disable the option to log in as root via SSH is to protect against possible attacks, as SSH brute-force attempts commonly target the root account since it is the most powerful user.

```
$ sudo nano /etc/ssh/sshd_config
$ PermitRootLogin no
$ sudo systemctl restart ssh
```

<div>
    <img src="/assets/images/77-KL-OmniVM_root-ssh-deactivation.png" style="width: 100%;">
</div>

<br>

## Configure and Enable UFW - Uncomplicated Firewall
UFW blocks all incoming traffic except what you explicitly allow — this is a basic and effective security step.<br>

1. Install UFW
2. Set up secure defaults
3. Enable UFW
4. Verify the status of it

```
$ sudo apt install ufw -y
$ sudo ufw default deny incoming
$ sudo ufw default allow outgoing
$ sudo ufw enable
$ sudo ufw status verbose
```

<div>
    <img src="/assets/images/77-KL-OmniVM_UFW-basic1.png" style="width: 100%;">
</div>

<br>

## Installing Essential Admin Tools
Why should one do that? Because Kali doesn’t include many useful admin tools by default. I installed them all in one go with a simple shell script:<br>

```
$ nano kali-admin-tools-setup.sh
```
```
#!/bin/bash

echo "[+] Updating package list..."
apt update

echo "[+] Installing useful admin tools (only those not yet installed)..."
apt install -y \
  iperf3 \
  glances \
  tmux \
  vnstat \
  traceroute \
  mtr \
  neofetch \
  mc \
  remmina \
  fail2ban \
  qemu-guest-agent \

echo "[+] Enabling QEMU Guest Agent..."
systemctl enable qemu-guest-agent

echo "[+] Setup complete. Tools and services installed and configured."
```
```
$ chmod +x kali-admin-setup.sh
$ sudo ./kali-admin-setup.sh
```

<div>
    <img src="/assets/images/77-KL-OmniVM_Essential-Admin-Tools-Setup.png" style="width: 100%;">
</div>

<br>

## Hardening Services and Brute-Force Protection
Disabling unnecessary services is a small adjustment, but it can have a significant impact depending on what you disable.<br>

1. List all running services
2. Disable those you don’t use, like Bluetooth

```
$ systemctl list-units --type=service
$ sudo systemctl disable bluetooth
```

<br>
Even though we installed and configured Fail2Ban in the script, I wanted to write it down again on how to do it separately, since brute-forcing is an important security aspect.<br>

```
$ sudo apt install fail2ban -y
```

<br>

## QEMU Guest Agent for Proxmox
Why QEMU Guest Agent? The QEMU Guest Agent gives Proxmox live insight into the guest OS, including:

- IP addresses (seen in the Proxmox GUI)
- Shutdown/reboot from the Proxmox interface
- Live memory/disk usage (when integrated)

Eventhough Proxmox has these kinds of options, it's much more smoother with the QEMU Agent than the default Proxmox GUI. A good YouTube video that explains this tool and the issues with Proxmox can be found [here](https://youtu.be/wp4kCUM6dik?feature=shared). The YouTuber explains it very well and provides a more detailed installation and explanation process than I do.<br>

1. Install the Agent.
2. Enable the service.
3. Configure Qemu in Proxmox GUI
   1. `OmniVM > Options`
   2. Edit QEMU Guest Agent
   3. Set to: `Use QEMU Guest Agent: Yes`
   4. Shutdown the VM, if you didn't already
4. In Proxmox Web UI, go to: `OmniVM → Hardware tab`
   1. Click Add → `Serial Port`
   2. Set: `Port: 0 (or leave default)` 
   3. **Do not change anything else**. Click Add
5. Start the VM again
6. Start the servie
7. Verify if it's running.

```
$ sudo apt update
$ sudo apt install qemu-guest-agent -y
$ sudo systemctl enable qemu-guest-agent
```
```
$ sudo systemctl start qemu-guest-agent
$ systemctl status qemu-guest-agent
```

<div>
    <img src="/assets/images/77-KL-OmniVM_QEMU-Guest-Agent-Setup.png" style="width: 100%;">
    <img src="/assets/images/77-KL-OmniVM_QEMU-Guest-Agent-Basic-Commands.png" style="width: 100%;">
</div>

<br>

***I won’t be explaining in detail what these commands do and how they work, but they are very useful and handy. The video linked above provides a thorough explanation.***


<br>


# Sources

- [ChatGPT](chatgpt.com)
- [QEMU Guest Agent - Comprehensive Breakdown](https://youtu.be/wp4kCUM6dik?feature=shared)