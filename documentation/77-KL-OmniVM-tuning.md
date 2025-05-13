# VLAN77 - Advanced Tuning of the Kali Linux Omni-VM

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



<div>
    <img src="" style="width: 100%;">
    <img src="" style="width: 100%;">
    <img src="" style="width: 100%;">
    <img src="" style="width: 100%;">
    <img src="" style="width: 100%;">
</div>