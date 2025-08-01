<div align="center">
  <div style="text-align: center;">
    <picture>
      <source media="(prefers-color-scheme: dark)" srcset="assets/images/stock/pantera-1.4.png">
      <source media="(prefers-color-scheme: light)" srcset="assets/images/stock/pantera-1.3.png">
      <img src="assets/images/stock/pantera-1.4.png" alt="Logo of Pantera" width="350px">
    </picture>
  </div>

  <br>

  [![Issues Badge](https://img.shields.io/badge/ISSUES-0-Test?style=for-the-badge&logo=https%3A%2F%2Ficons8.com%2Ficon%2F83178%2Fimage-file&labelColor=%23333333&color=%23ba181b)](https://github.com/callme-pantera/CSL-prototype/issues)
  [![Stars Badge](https://img.shields.io/badge/STARS-1-Test?style=for-the-badge&logo=https%3A%2F%2Ficons8.com%2Ficon%2F83178%2Fimage-file&labelColor=%23333333&color=%23f6aa1c)](https://github.com/callme-pantera/CSL-prototype/stargazers)
  [![Commits Badge](https://img.shields.io/github/commit-activity/m/callme-pantera/CSL-prototype?style=for-the-badge&label=COMMITS&logo=https%3A%2F%2Ficons8.com%2Ficon%2F83178%2Fimage-file&labelColor=%23333333&color=%237678ED)](https://github.com/callme-pantera/CSL-prototype/commits/main/)
  [![License Badge](https://img.shields.io/badge/LICENSE-CC-Test?style=for-the-badge&logo=https%3A%2F%2Ficons8.com%2Ficon%2F83178%2Fimage-file&labelColor=%23333333&color=%234361ee)](LICENSE)
</div>

<br>

# Overview
In this project, I will document the process of creating a **C**yber**s**ecurity **L**ab at home. The goal is to establish an environment protected from potential attacks or exploits that an external malicious user might try to take advantage of. I will be building this from scratch and will make sure to document every single step, as I believe this will help me reflect better when I want to update or change something. 
<br><br>
This is not a tutorial or guide, just a documentation of my work and progress, the way I approach it.
I want to mention that you don’t have to follow every step exactly as I did. I’ll be documenting everything, including some details that may not be strictly necessary.
<br><br>
The documentation for the entire project can be found [here](/documentation/README.md), at the bottom in the [Summary](#summarization) or in the **documentation** folder at the top of the page. Additionally, if you would like to read through and understand my troubleshooting processes, you can find them in the **troubleshooting** folder at the top of the page as well.


<br>

> [!NOTE]
> It is possible that my documentation and project may contain some mistakes or errors. Therefore, some prior knowledge of the topic is necessary to ensure clarity and avoid confusion.

<br>

## Concept
Before starting, I created a prototype of the network plan using [Draw.io](https://app.diagrams.net/). The goal was to visualize my ideas and ensure that everything could be connected logically. I didn’t focus much on design, as I wanted to confirm the plan’s feasibility before refining its appearance.

These are some of the points I wanted to include in my Cybersecurity Lab:

- Containers  
- Testing environment  
- Firewall and monitoring tools  
- Private network
- VLANs  
- Kali Linux
- Proxmox

<br>

![network plan CSL](/assets/images/CSL-network-plan-v1.3.png)

<br>

## What do you need?
Throughout this project, I will be adding to and adjusting my documentation as needed. For now, here are the essential components you'll need:<br>

| **What**                                                              | **Why**                                                               |
|:----------------------------------------------------------------------|:----------------------------------------------------------------------|
|[Proxmox](https://www.proxmox.com/en/)                                 |*Proxmox* is a virtualization platform that lets you run and manage multiple VMs and containers on a single physical server. I chose *Proxmox* because of its flexibility and ease of use, but there are alternatives like *VMware ESXi* or *VirtualBox* depending on your needs.|
|[OPNsense](https://opnsense.org/download/)                             |A firewall is used to control inbound and outbound traffic, as well as various other settings such as VLAN configurations etc. I chose *OPNsense*, but you could also use *pfSense* or any other firewall that suits your needs.|
|[Ubuntu Server](https://ubuntu.com/download/server)                    |I chose *Ubuntu Server* because I’m familiar with it and wanted to use it to host my containers.|
|[Kali Linux](https://www.kali.org/get-kali/#kali-platforms)            |*Kali Linux* is a powerful penetration testing operating system, equipped with numerous pre-installed tools that are highly useful for my project.|
|[Ubuntu Desktop](https://ubuntu.com/download/desktop)                  |I chose *Ubuntu Desktop* for testing purposes, but any other operating system would work just as well.|
|[Windows](https://www.microsoft.com/en-us/software-download/windows11) |I chose *Windows* for testing purposes, but any other operating system would work just as well.|
|||

<br>

# Summarization
A summary of the entire project's contents:

- [The Main Project Documentation](documentation/README.md)
  - [VLAN77 - OmniVM Tuning](documentation/77-KL-OmniVM-tuning.md)

<br>

- [Troubleshooting 1.1 - Proxmox / Laptop issue](troubleshooting/TS-proxmox-laptop-issue.md)
- [Troubleshooting 1.2 - Linux Bridge missing](troubleshooting/TS-vmbr2-missing.md)
- [Troubleshooting 1.3 - WireGuard-VPN issue](troubleshooting/TS-WireGuard-VPN-for-VM.md)



<br>


# Sources

- YouTube:  [Gerard O'Brien, Building the Ultimate Cybersecurity Lab](https://www.youtube.com/watch?v=XIvn0ZDSmKA&list=PL3ljjyal211AbTqlxSo6CGBiVqsXw8wrp&index=22)
- Medium:   [TheInfoSec Guy](https://medium.com/@jibingeorge.mg/cybersecurity-research-lab-setup-5beb54d8dd59)
- ChatGPT:  [OpenAI](https://chatgpt.com/)
- Friends

