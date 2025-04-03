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

# Installation and configuration
Before I actually began the project, I wanted to install and configure all the necessary software, ISO files, etc., so that everything was ready for the CSL. The goal was to prepare everything in advance and save time during the different stages of the project.

## Proxmox
For the Proxmox installation and configuration, I used the [documentation](https://www.proxmox.com/en/proxmox-virtual-environment/get-started) provided by Proxmox themselves and followed it step by step. Keep in mind to read carefully and not just click through—take the time to understand each step.

<br>

> [!CAUTION]
> Installing the Proxmox ISO Installer will permanently overwrite the disk it is installed on, as it is a bare-metal installer. This means that any existing data will be permanently removed. Proceed with caution!

<br>

You should be able to install it on your own, as the documentation is quite clear. Here are the steps I followed for the installation:

- Download the Proxmox ISO image installer (*in my case the 8.2-2 version*)
- Download [Rufus](https://rufus.ie/de/) or another USB burning tool
- Burn the Proxmox ISO onto a USB drive with at least 8GB of free space
- I used my old laptop to boot Proxmox from the USB drive --> BIOS
- Proceed with the Proxmox installation

<br>

> [!WARNING]  
> As mentioned earlier, burning this disk with the Proxmox ISO file will permanently erase **ALL DATA** on the selected disk!


<h4>Rufus Setup</h4>
<div style="display: flex; justify-content: space-between;">
  <img src="/assets/images/rufus-1.png" alt="Rufus 1" style="width: 45%;" />
  <img src="/assets/images/rufus-2.png" alt="Rufus 2" style="width: 45%;" />
</div>

<br>

<h4>Proxmox Booting via USB Drive - 1st Attempt</h4>
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

