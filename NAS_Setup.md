# NAS Setup Guide

## Overview

This guide explains how to set up the Network Attached Storage (NAS) portion of the home server.  
The NAS provides centralized file storage accessible over the local network via Samba.

---

## Prerequisites

Before starting, ensure the server has:

- Linux installed (Ubuntu/Debian recommended)
- Storage drives mounted (SSD1, SSD2)
- Root or sudo access

Verify drives are connected:

lsblk

NAME   SIZE TYPE MOUNTPOINT
sdb1   1T   disk 
sde2   1T   disk 

## Mounting Drives

Follow these steps to mount your storage drives so they are automatically available for Plex and Samba:

### Identify your drives

Run:

lsblk -f

Look for the drives you want to use. Example output:

NAME   FSTYPE LABEL UUID                                 MOUNTPOINT
sda    ext4         245634dd-b9d3-4c6f-a874-f9c60eb31e7
sdb    exfat        CC58-00EF
sdc    exfat        0421-DC98

Here, we will use the exFAT drives CC58-00EF and 0421-DC98.

### Create mount points

Create directories where the drives will be mounted:

sudo mkdir -p /mnt/ssd1
sudo mkdir -p /mnt/ssd2

### Edit fstab for automatic mounting

Open /etc/fstab in your preferred editor:

sudo nano /etc/fstab

Add the following lines at the end of the file (replace UUIDs with the ones from Step 1):

UUID=CC58-00EF /mnt/ssd1 exfat defaults,nofail,uid=1000,gid=988,fmask=002,dmask=002 0 0
UUID=0421-DC98 /mnt/ssd2 exfat defaults,nofail,uid=1000,gid=988,fmask=002,dmask=002 0 0

uid and gid should match the Linux user who will access the drives

fmask and dmask control file and folder permissions

nofail prevents boot errors if a disk is missing

Save and close the file (Ctrl+O → Enter → Ctrl+X).

### Test the mounts

Apply the fstab changes immediately:

sudo mount -a

Check that the drives are mounted:

df -h | grep /mnt

Expected output:

/dev/sdb   1T  /mnt/ssd1
/dev/sdc   1T  /mnt/ssd2

## Installing Samba

Install Samba to share the NAS folders over the network:

sudo apt update
sudo apt install samba

### Configure Samba Shares

Open the Samba configuration file:

sudo nano /etc/samba/smb.conf

Add your shares at the bottom of the file:

[Movies]
   path = /mnt/ssd1
   browsable = yes
   writable = yes
   guest ok = no
   valid users = user

[TVShows]
   path = /mnt/ssd2
   browsable = yes
   writable = yes
   guest ok = no
   valid users = user

Adjust [Movies] and [TVShows] as desired.
valid users restricts access to authorized accounts.

Save and close the file.

### Create Samba User

Create a user for accessing the NAS:

sudo adduser user
sudo smbpasswd -a user

Replace user with your preferred username.

This user will be used to access Samba shares from other devices.

### Set Permissions

Ensure that the NAS folders are accessible by the Samba user and Plex:

sudo chown -R user:user /mnt/ssd1
sudo chown -R user:user /mnt/ssd2
sudo chmod -R 775 /mnt/ssd1 /mnt/ssd2

chown ensures ownership is correct

chmod 775 allows read/write for owner and group, read-only for others

7. Start and Enable Samba

Enable Samba so it starts automatically on boot:

sudo systemctl start smbd
sudo systemctl enable smbd

Check status:

sudo systemctl status smbd
8. Accessing NAS Shares
Windows

Open File Explorer → Map Network Drive

Enter the server path:

\\SERVER-IP\Movies
\\SERVER-IP\TVShows

Authenticate with the Samba username and password you created.

Linux
smbclient //SERVER-IP/Movies -U user
smbclient //SERVER-IP/TVShows -U user
9. Testing

Copy a test file into each share

Verify read/write access from another machine on the LAN

Ensure Plex or other media servers can access /mnt/ssd1 and /mnt/ssd2 for media libraries

10. Best Practices

Keep uid and gid consistent for Plex and NAS users

Use descriptive mount points (/mnt/ssd1, /mnt/ssd2)

Backup critical media regularly

Use UUIDs in fstab to prevent accidental drive swapping