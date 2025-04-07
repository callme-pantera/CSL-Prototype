# Troubleshooting - vmbr2 missing from interfaces file
As mentioned before, I had to update the `/etc/network/interfaces` file to define the configuration for *vmbr2*. The correct configuration is as follows:

<br>

```
auto vmbr2
iface vmbr2 inet static
    address x.x.x.x
    netmask 255.255.255.0
    bridge-ports none
    bridge-stp off
    bridge-fd 0
    pre-up iptables-restore < /etc/iptables.up.rules
```

```
$ ifreload -a
```

<br>

<div>
<img src="/troubleshooting/assets/images/vmbr2-missing.png" style="width: 100%;">
<img src="/troubleshooting/assets/images/vmbr2-updated_network-reload.png" style="width: 100%;">
</div>

<br>

After the network reboot, you can start the OPNsense VM as you normally would :)

<br>

# Sources

- ChatGPT:  [OpenAI](https://chatgpt.com/)