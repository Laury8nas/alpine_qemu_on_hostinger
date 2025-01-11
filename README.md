# How to create Alpine Linux VM on QEMU for Hostinger Shared hosting plan
This guide will show how it's possible to create Alpine Linux 3.19  on a Hostinger Shared/Cloud hosting plans without root access. This method will be based on [Miniconda](https://docs.anaconda.com/miniconda/ "Miniconda") usage and prebuild [QEMU](https://github.com/qemu/qemu "QEMU") 9.2.50 release.

## Preparation

Before starting, you need to make sure that:
- You have access to your [hosting plan via SSH](https://support.hostinger.com/en/articles/1583245-how-to-connect-to-a-hosting-plan-via-ssh "hosting plan via SSH")
- Have [a basic knowledge](https://www.hostinger.com/tutorials/linux-commands "a basic knowledge") of using Linux terminal

## Installation of Alpine Linux VM

Since the process of installing the Alpine Linux VM on a shared/cloud hosting plan requires a lot of packages to be installed, I simplified the whole process by preparing a bash script that will do the whole "heavy lifting" for you.

First, you need to be connected to your hosting plan via SSH and execute this command (remember to review the script before executing!):
```bash
wget https://github.com/Laury8nas/alpine_qemu_on_hostinger/raw/refs/heads/main/create_alpine_vm.sh && chmod +x create_alpine_vm.sh && ./create_alpine_vm.sh
```

For me, on the Premium hosting plan it took about 3 minutes and 10 seconds to see the login screen on my terminal for Alpine Linux after the installation process, but it may vary in every case.
![2025-01-11_21-32](https://github.com/user-attachments/assets/77af0924-843c-41a4-8cd5-93643d61b928)
*Note: If you're stuck on the blank screen for a long time (more than 1 minute), make sure to press "Enter" a couple of times.*

**The default credentials for Alpine Linux are:**
- Username: root
- Password: Hostinger2025+

When you want to shutdown your Alpine Linux VM, just type:
```bash
poweroff
```

## Opening Alpine Linux VM after installation

If you already created the Alpine Linux VM and want to relaunch it later, then you can simply execute this command to open the already created VM:
```bash
wget https://github.com/Laury8nas/alpine_qemu_on_hostinger/raw/refs/heads/main/open_alpine_vm.sh && chmod +x open_alpine_vm.sh && ./open_alpine_vm.sh
```

**A few things to know:**
- All the changes that you make in the VM will be saved to the virtual disk which can be found at this location:  `~/alpine/alpine-virtual-backup.qcow2`;
- The created virtual disk maximum size is set to 16 GB;
- The QEMU will use a maximum of 1024 MB of RAM from the host machine for that VM by default;
- The QEMU will use a 1 CPU core from the host machine for that VM by default;
- The QEMU will use `user` network mode for the created VM which provides a basic network/Internet functionality, but no communication between host and VM unless specified differently in the script;
- Make sure to be familiar with [apk package manager](https://wiki.alpinelinux.org/wiki/Alpine_Package_Keeper "apk package manager") as Alpine Linux provides really minimal set of packages installed by default;
- Be aware that the Hostinger server kills processes automatically that are running for more than 60 minutes, so if you plan to use VM for a longer time, you will need to re-launch it manually (by using my VM opening script) or via cron-job;
- Be aware that QEMU doesn't use hardware acceleration and runs as a regular process in the hosting plan, so VM power is limited to something about 10-30% of what the host machine can do in reality and the QEMU will put a lot of stress on the server's resource usage as well.

## Final thoughts

It was a really fun project to do! I learned a lot about QEMU itself by trying to make it work on the Hostinger's shared hosting plan without root access. Virtualization opens a lot of potential, like sharing files/folders between host and VM via the [9P](https://wiki.qemu.org/Documentation/9p "9P") protocol, processing them inside the VM and giving them back to the host, configuring network settings, doing port forwarding between host and VM, etc. However, the QEMU usage without KVM is really limited in processing power on shared hosting plans because you're constantly hitting the resource usage limits and if you try to run something, like a WordPress website in a VM, then the page loading time can take 4-20 seconds. So, you need to be creative and think responsibly about the tasks that you want to put on the QEMU Alpine Linux VM.
