---
layout: post
title: Synology Settings for NFS 
description: A quick run through of the settings required to open up your Synology NAS for mounting via NFS.
categories: 
  - synology
  - linux
  - tutorial
comments: true
---
I've been using a Synology NAS on my home network for several years. They make a great product for anyone who wants a simple NAS with lots of flexibility. I use it to pass files between systems on my network quite a bit. Here's a quick walkthrough of settings within Synology's DSM to enable mounting the NAS over the network.  
<!--more-->

Synology allows you to create Shared Folders each with its own settings for permissions, sharing, encryption, ect. You can think of these as separate logical volumes all on the same NAS device. I've got shares for backups, files, photos, Plex, and a home share for each user.

*For these steps, I'm running Synology DSM version 6.2.2-24922 Update 4 on a DS216j. Most of the guides on Synology's own site are outdated, referencing DSM 5 or earlier.*

In this example, I'll be creating a shared folder called `backups`, granting NFS permissions for and mounting it to my RaspberryPi.

1. [Create the Shared Folder](#create-the-shared-folder)
2. [Edit Shared Folder NFS Permissions](#edit-shared-folder-nfs-permissions)
3. [Enable NFS Access through the Synology Firewall](#enable-nfs-access-through-the-synology-firewall)
4. [Mount NAS folder to the Pi](#mount-nas-folder-to-the-pi)


## Create the Shared Folder  

Navigate to the Synology DSM, open the Control Panel and select Shared Folder.
  - Click Create and follow the Wizard prompts. I typically agree to the default values with the exception of disabling the Recycle Bin.
    ![create-share]({{ site.img_dir}}create-share.png)

## Edit Shared Folder NFS Permissions  

Once the folder has been created, we have to allow NFS access for the RaspberryPi to connect to the shared folder on the Synology NAS. We do this by adding an NFS rule mapped to the IP address of the Pi (`192.168.1.62`).
  - Select the `backups` Shared folder and click Edit > NFS Permissions.
  ![control-panel]({{ site.img_dir }}control-panel.png)
    - **Note:**Make note of the folder mount path at the bottom left of this window. We'll need this later.  
    ![mount-path]({{ site.img_dir }}mount-path.png)
  - Click Create and match these settings (changes in **bold**):
    - **Hostname or IP*: Enter the local IP address (mine is `192.168.1.62`)**
      - *If you want to open access to this folder from all machines on your local network, use the `*` wildcard instead of the specific LAN IP address.*
    - Privilege: Read/Write (default)
    - **Squash: Select "Map root to admin" from the drop down menu**
    - Security: sys (default)
    - Select Enable asynchronous (default)
    - Deselect Allow connection from non-privileged... (default)
    - **Select the checkbox for "Allow users to access mounted subfolders"**
  ![nfs-rule]({{ site.img_dir }}nfs-rule.png)
  - Click OK. Click OK. 

## Enable NFS Access through the Synology Firewall  

If you have the Synology NAS built in firewall active, you'll need to enable rules to allow the required NFS ports. *If your firewall is disabled, you can skip this step.*

- Check your firewall status in the DSM Control Panel under the Security > Firewall tab.
  ![cp-firewall]({{ site.img_dir }}cp-firewall.png)
- Click Edit Rules > Create 
- Under Ports choose Select from a list of built-in applications and click Select. Find Mac/Linux file server and check the box to enable. 
  ![firewall-nfs]({{ site.img_dir }}firewall-nfs.png)
- If you plan to use this shared folder with Windows machines as well, enable the Windows file server too. 
- Click OK.
- Under Action, select Allow.
- Click OK.
- Make sure the Enabled check box is checked for the created rule, and Click OK.
- Click Apply.

## Mount NAS folder to the Pi

- First, update the local package index and install `nfs-common`.
```bash
$ sudo apt update
$ sudo apt install nfs-common
```
- Create a folder on the Pi that will be used as a mount point where we will attach the NAS. This can be anywhere on your system. Typically, mounted volumes are placed in the `/mnt` folder. I'm keeping it simple here.
  ```bash
  $ sudo mkdir /mnt/backups
  ```
- Attach the NAS shared folder 

  ```bash
  $ sudo mount <nas.ip.address>:[/share/mount-path] [/mnt/point]
  ```

  Using my values, here's the script I executed. Replace these with your values. No response is expected from this command.
    - Remember the mount path from earlier? - `/volume1/backups` 
    - RaspberryPi mount point - `/mnt/backups`
    - NAS IP - `192.168.1.30`.

      ```bash
      $ sudo mount 192.168.1.30:/volume1/backups /mnt/backups
      ```

  - If you get a `no such file or directory` error, make sure you're using `sudo` and double check the IP address, NAS mount path and local mount point.
  - if you get a `Timed out` error, check your [Synology firewall](#enable-nfs-access-through-the-synology-firewall) settings.  

 - Verify the mount was successful by checking the attached disks (`df`) and searching for the mount point we created on the Pi (`grep /mnt/backups`). 
  ```bash
  $ df -h | grep /mnt/backups
  ```
  - If successful, it will return something like this: 
  ```bash 
  192.168.1.30:/volume1/backups  5.4T  3.7T  1.8T  68% /mnt/backups
  ```

## Done

Not too bad! You now have access to the shared folder to use it however you need. You can also mount other shares by creating new mount points and following the same set of instructions to enable NFS access. Using the same RaspberryPi and Synology NAS, I have `/mnt/backups` and `/mnt/files` attached for different purposes. Neat!

Let me know if you have questions or if there's something I missed.