# arch-install

Boot the arch install media then
```
wget "https://raw.githubusercontent.com/jplikesbikes/arch-install/master/base-install"
chmod +x base-install
./base-install
```

## Resizing a partition
From jack:
```
Here's the quick overview of how I resized `/var`. Feel free to pare it down or whatever

Resize /var:
Determine the volume group name: `sudo lvm vgdisplay`
Determine the logical volume name: `sudo lvm lvdisplay`
Since you can't resize the volumes while using them, we need to enable root: `sudo passwd -u root`
Restart: `sudo shutdown -r now`
Before logging in, open a shell (Ctrl + Alt + F2)
Log in as root
Resize the /var partition (using the volume group and logical volume names). My volume group is MyStorage and logical volume is varvol: `lvresize --resizefs -L +10G MyStorage/varvol`
You may need to shrink another volume to make room for this: `lvresize --resizefs -L -10G MyStorage/homevol`
Restart again: `shutdown -r now`
Log in normally
Remember to disable root (if you want): `sudo passwd -l root`
```

## decrypting a second harddrive on boot
https://www.golinuxcloud.com/mount-luks-encrypted-disk-partition-linux/

## Clean the systemd journals
Keeps the last week of logs
```
journalctl --vacuum-time=7d
```

## Per user file limits
add the following to `/etc/security/limits.conf`
```
* hard nofile 65536
```

## downgrade packages
https://wiki.archlinux.org/index.php/Arch_Linux_Archive#How_to_restore_all_packages_to_a_specific_date
```
or by replacing your /etc/pacman.d/mirrorlist with the following content:
##                                                                              
## Arch Linux repository mirrorlist                                             
## Generated on 2042-01-01                                                      
##
Server=https://archive.archlinux.org/repos/2014/03/30/$repo/os/$arch
Then update the database and force downgrade:
# pacman -Syyuu
```

### Mods for make package

Can speed it up by using all threads or disabling compression:
https://gist.github.com/frandieguez/0b13bd58148679aa9955

I noticed that compressing packages hangs the GNOME desktop. Instead of disabling it, I could just go all-in: https://wiki.archlinux.org/index.php/makepkg#Utilizing_multiple_cores_on_compression

## Global Environment variables
/etc/environment
```
_JAVA_AWT_WM_NONREPARENTING=1
VK_ICD_FILENAMES=/usr/share/vulkan/icd.d/intel_icd.x86_64.json
```
## Nvidia card 
+ set kernel parameter ibt=off
+ don't use nvidia for graphics only cuda
/etc/modprobe.d/nvidia.conf
```
blacklist nvidia_drm
blacklist nvidia_modeset
blacklist nouveau
```
+ install 'nvidia' for default kernel, 'nvidia-lts' for lts-kernel
