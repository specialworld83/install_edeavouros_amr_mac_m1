## Video install 
https://youtu.be/Y2mFeuVMagA

## Useful links
https://wiki.archlinux.org/title/Install_Arch_Linux_from_existing_Linux#Using_a_chroot_environment
https://archlinuxarm.org/platforms/armv8/generic
https://arm.endeavouros.com/endeavouros-arm-install/

## Format partition boot
mkfs.fat /dev/sdb1
## Format partition root
mkfs.ext4 /dev/sdb2

## Mount partition
mount /dev/sdb2 /mnt

## Download archlinux arm
cd /mnt
wget http://os.archlinuxarm.org/os/ArchLinuxARM-aarch64-latest.tar.gz
bsdtar -xpf ArchLinuxARM-aarch64-latest.tar.gz
rm -rf ArchLinuxARM-aarch64-latest.tar.gz

## Get into chroot
mount --bind /mnt /mnt && cd /mnt && rm /mnt/etc/resolv.conf && cp /etc/resolv.conf etc && mount -t proc /proc proc && mount --make-rslave --rbind /sys sys && mount --make-rslave --rbind /dev dev && mount --make-rslave --rbind /run run
chroot /mnt /bin/bash
## prepare multiple download
nano /etc/pacman.conf
## Search parallel and remove flag #
ParallelDownloads = 5

## Init pacman keyring
pacman-key --init

pacman-key --populate archlinuxarm

## Commands for install
pacman -Syu base linux linux-firmware vim arch-install-scripts efibootmgr networkmanager network-manager-applet dialog os-prober mtools dosfstools base-devel linux-headers


## prepare boot
ls /boot
cp -rv /boot/* /tmp/
mount /dev/sdb1 /boot ## mount boot efi partition 
cp -rv /tmp/* /boot/

## config system
dbus-uuidgen > /etc/machine-id ## Fix for missing machine-id 
genfstab / >> /etc/fstab ## Check your fstab after issuing this command to make sure there aren't other partitions 
ln -sf /usr/share/zoneinfo/Europe/Rome /etc/localtime ## This is if you're located in Europe with Rome's timezone
hwclock --systohc
nano /etc/locale.gen ## Select your locales
locale-gen
echo "LANG=it_IT.UTF-8" >> /etc/locale.conf ##Assuming you want this locale
echo "endeavouros" >> /etc/hostname
nano /etc/hosts ## Here we use vim to modify the hosts file

## ADD MANUAL IP
127.0.0.1        localhost
::1              localhost

## Install bootloader
bootctl --path=/boot install ## Here we begin setting up systemd-boot 

## Enable NetworkManager
systemctl enable NetworkManager



### Configs

nano /boot/loader/loader.conf
timeout 10
#console-mode keep
default arch-*

nano /boot/loader/entries/entries.arch.conf
title	EndeavourOS
linux	/Image
initrd	/initramfs-linux.img
options root=/dev/sdb2 rw


## Download script in /opt
cd /opt
## For endeavourOS
git clone https://github.com/endeavouros-arm/install-script.git


## Exit ssh
Create system into parallel

## System Debian 
halt system

## Create VM 
https://youtu.be/Y2mFeuVMagA
## Start EndeavouOS 

cd /opt/install-script
chmod 777 endeavour-ARM-install-V2.X.sh


