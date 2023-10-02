---
title: "Introduction to ZFS on Linux"
date: 2023-08-30T20:42:02+01:00
draft: false
params:
    ShowShareButtons: true
---
- January 13, 2022  

### Overview  
Developed in 2001 by Sun Microsystems - not GNU licensed so has to be installed on linux separately.

### Main features are:  

Copy-on-write  
Snapshots  
Pooled storage  
Data integrity verification and automatic repair  
RAID-Z  
Maximum 16 Exabyte file size  
Maximum 256 Quadrillion Zettabytes storage  

### Installation on fedora 35:  
dnf update   
dnf install -y kernel-devel  
dnf install -y zfs    
modprobe zfs 
 
Now I had already attached a physical disk to my desktop machine that had a zfs pool on it.To make this available I ran:  

zpool import pool: studat  
id: 2784278569668224000
state: ONLINE
status: The pool was last accessed by another system.
action: The pool can be imported using its name or numeric identifier and the '-f' flag.  see: https://openzfs.github.io/openzfs-docs/msg/ZFS-8000-EY  zpool import -f studat

we can now see my zfs volume mounted and available:  
df -t zfs  
Filesystem     1K-blocks     Used Available Use% Mounted on  
studat         690841728 43576576 647265152   7% /studat
  
### Common Commands  
zpool status -x                                 -> shows pool status  
zpool list                                      -> list all pools  
zpool list -o name,size                         -> show specific pool   properties  
zpool create stupool dev                        -> create a pool on device dev  
zfs create -V 2gb stupool/stuvol           -> create a 2gb volume on stupool  
zfs set mountpoint=/stu stupool/stuvol -> change mountpoint for filesystem  
zpool iostat 2                                        -> show zfs io stats every 2s  
zpool scrub stupool                              -> run a scrub on all   filesystems on this pool
zpool online                                          -> online a disk to clear error count  
zpool import                                          -> list pools we can import  
zpool export stupool                             -> export stupool  
zfs list -t snapshot                                 -> list snapshots  
zfs rename studat/stuvol studat/stufs1  -> rename zfs fs  
zfs snapshot studat/stufs1@17012101132  -> create a snapshot  
zfs list -t snapshot                                        -> list snapshots  
NAME                                     USED  AVAIL     REFER  MOUNTPOINT  
studat/stufs1@17012101132     0B      -             96K           -  

