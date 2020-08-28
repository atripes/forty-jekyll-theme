---
layout: post
title: Resize a logical volume in Linux
description: This comes in handy, if you have to deal with LVMs on virtual machines.
permalink: :title
---

You just resized (this post only handles the adding of space) the disk in say XenServer. Now, on your VM there is a mounted disk with partitions on it.
Take a look with `fdisk -l`

This writeup assumes an Extended Partition (2) and a LVM partition (5). The name of the volume group is found by `vgdisplay`.

Commands that come in handy during the whole operation:
  * `pvdisplay` prints information about the physical volumes on your device
  * `vgdisplay` prints information about the volume group on your device
  * `lvdisplay` prints information about the logical volumes on your device

The idea is, when resizing (eg. making it bigger in our case) that you recreate your partitions in `fdisk`. Be sure to change the partition types to the ones needed: Extended (5) for the primary partition and LVM (8e) for the logical partition. Once you recreated those partitions with maximum sectors - now it should be as much bigger as you wanted it to be - you can resize your physical volume. A restart might be necessary to inform your kernel of the changed properties of `/dev/xvdx`. At this point I always get the error 
```
Failed to initialize DM devices
```
 right at shutdown. I don't know what that means, it has to do with Kernel and LVMs, but for me it was totally fine to ignore this, since after we are done, everything works fine.  
So, after the reboot you can then  
```
pvresize /dev/xvda5
```
I assume my own environment of `/dev/xvda5`, yours may differ. The partition name, of the partition you want to resize (the one recreated earlier), is found with `fdisk -l`
Hit `pvdisplay` again to show the new size. Also `vgdisplay` now knows of your newly added space.
Now you can add the space to your logical volumes:  
```
lvresize -l+100%FREE /dev/<vg_name>/<lv_name>
```
You can find the names for the volume group (vg) and the logical volume (lv) with the respective `display` commands. 
And then resize the fs:   
```
resize2fs /dev/<vg_name>/<lv_name>
```

You should be done. Check with `df -h`.

This small graphic helped me understand LVMs much better.
(Thanks for that Mr. [pe-kay](https://pe-kay.blogspot.com/2013/04/linux-lvm-explained.html))
{: style="text-align:center"}
![Sensei](/assets/images/lvm_explained.png)
