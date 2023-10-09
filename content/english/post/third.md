---
title: "How to grow a volume on an EC2 instance"
date: 2023-07-02T19:50:32+01:00
Description: ""
Tags: []
Categories: []
DisableComments: false
---
 How to grow a volume on aws ec2 instance

so - space getting a little tight on box, cant archive or delete anything - lets grow it.
another inherited box - i would really have preferred app dir to be on a separate partition to root, but moving this one required downtime so lets do it whilst our devs are still using it!

prior to growing:
# df -hl
/dev/xvda1       36G   32G  4.1G  89% /

# lsblk
NAME    MAJ:MIN RM  SIZE RO TYPE MOUNTPOINT
xvda         202:0    0   32G  0 disk
└─xvda1 202:1    0   36G  0 part /

we then go into aws console, look at instance under ec2 instances, click on actions -> modify volume and then increase size to 100G.
alternatively (ideally!) use terraform to increase volume size, or cloudformation or:

aws ec2 modify-volume --size 100 --volume-id vol-yourvolidhere


df remains the same but lsblk now shows:
# lsblk
NAME    MAJ:MIN RM  SIZE RO TYPE MOUNTPOINT
xvda         202:0    0  100G  0 disk
└─xvda1 202:1    0   36G  0 part /
we can extend the space onto partion 1 by:
# growpart /dev/xvda 1
CHANGED: partition=1 start=4096 old: size=75493343 end=75497439 new: size=209711071 end=209715167
# lsblk
NAME    MAJ:MIN RM  SIZE RO TYPE MOUNTPOINT
xvda    202:0    0  100G  0 disk
└─xvda1 202:1    0  100G  0 part /

we now need to grow our partition, we get the partion type by:

# df -hlT
/dev/xvda1     xfs        36G   32G  4.1G  89% /

so we know its an xfs partition.

we need the tool xfs_growfs which is provided by xfsprogs - either add this ideally via your config management tool (ansible, puppet etc) or manually:

# yum -y install xfsprogs

we can then grow our partition:
# xfs_growfs -d /
......
data blocks changed from 9436667 to 26213883

# df -hl
Filesystem      Size  Used Avail Use% Mounted on
/dev/xvda1      100G   33G   68G  33% /


job done!
