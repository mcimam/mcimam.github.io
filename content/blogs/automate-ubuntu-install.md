+++
title = 'Automate Ubuntu Install'
tags = ['devops', 'ubuntu']

date = 2024-07-07T20:30:09+07:00
draft = false
showtoc = false
hideSummary = true
disableShare = true
+++

# How to create ubuntu autoinstall

Creating an automated installation ISO for Ubuntu can save time and ensure consistency across deployments. Here's a step-by-step guide to create your own autoinstall ISO. Automatic Ubuntu installation is performed with the autoinstall format. You might also know this feature as “unattended”, “hands-off” or “preseeded” installation.

This format is supported in the following installers:
- Ubuntu Server, version 20.04 and later
- Ubuntu Desktop, version 23.04 and later

## Step 1 : Download ubuntu package
First, download the latest Ubuntu live server ISO from the official website.
Download [here](https://ubuntu.com/download)


## Step 2: Create file ´autoinstall-user-data´
Next, create a file named autoinstall-user-data. Here's an example configuration:
``` yml
#cloud-config
autoinstall:
  version: 1
  identity:
    hostname: svr
    password: "$y$j9T$eKDQKBfE.NOfLgQCzYNLB1$nmJbonWd.WtWoCKMtbTD4HlLK6lEkFj3AI378PDOw23"
    username: mcimam
  ssh:
    allow-pw: true
    install-server: true
  shutdown: reboot
```
For more detail check [Ubuntu Autoinstall Ref](https://canonical-subiquity.readthedocs-hosted.com/en/latest/reference/autoinstall-reference.html)

## Step 3: Extract iso 
Extract the contents of the ISO file using the following commands:
``` sh
mkdir source-files
xorriso -osirrox on -indev ubuntu-24.04-live-server-amd64.iso --extract_boot_images source-files/bootpart -extract / source-files
```

## Step 4: Add auto install configuration to iso
Add the autoinstall-user-data file to the extracted files:

```
cd source-files
mkdir nocloud
cd nocloud
cp ../../autoinstall-user-data user-data
touch meta-data
```

## Step 5. Add autoinstall menu to grub
Edit the GRUB configuration file to add an autoinstall menu entry:
``` sh
vi source-files/boot/grub/grub.cfg
```

Add following menu entry:
``` cfg
# Add this one
menuentry "Autoinstall Ubuntu Server" {
    set gfxpayload=keep
    linux   /casper/vmlinuz quiet autoinstall ds=nocloud\;s=/cdrom/nocloud/  ---
    initrd  /casper/initrd
}
```

## Step 6. Repack iso file
Finally, repack the ISO with the following command:

```
xorriso -as mkisofs -r -V "ubuntu-autoinstall" -J -boot-load-size 4 -boot-info-table -input-charset utf-8 -eltorito-alt-boot -b bootpart/eltorito_img1_bios.img -no-emul-boot -o ../ubuntu-24.04-server-amd64-autoinstall.iso .
```
Now you have a custom Ubuntu autoinstall ISO ready to use!


## Video Reference
This note is based on tutorial provided by ubuntu and DanielPersson.

Full video reference from DanielPersson.

{{< youtube DtXZ6BMaKbA >}}

