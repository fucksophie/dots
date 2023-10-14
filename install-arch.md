# yourfriend's arch installscript

This does not do the partitioning of your system. [Look at the example layouts](https://wiki.archlinux.org/title/Partitioning#Example_layouts) for Archlinux, and apply your changes with `cfdisk`.

We are assuming you want to use BTRFS, NetworkManager, linux-zen, and UEFI/GPT.

``` 
# create filesystems
mkfs.fat -F 32 /dev/sda1
fatlabel /dev/vda1 ESP
mkswap -L SWAP /dev/vda2
mkfs.btrfs -L ROOT /dev/vda3

# enables swap, and mounts filesystems
swapon /dev/disk/by-label/SWAP   
mount /dev/disk/by-label/ROOT /mnt
mkdir /mnt/boot
mkdir /mnt/boot/efi
mkdir /mnt/home
mount /dev/disk/by-label/ESP /mnt/boot/efi

# generates minimum to chroot
pacstrap -K /mnt base linux-zen linux-firmware nano networkmanager grub efibootmgr btrfs-progs
genfstab -U /mnt >> /mnt/etc/fstab

# chroot and setup time
arch-chroot /mnt
ln -sf /usr/share/zoneinfo/Europe/Riga /etc/localtime
hwclock --systohc

# setup locale
sed -i "s/#en_US.UTF-8 UTF-8/en_US.UTF-8 UTF-8/" /etc/locale.gen
sed -i "s/#lv_LV.UTF-8 UTF-8/lv_LV.UTF-8 UTF-8/" /etc/locale.gen
locale-gen
echo "LANG=en_US.UTF-8" > /etc/locale.conf

# set hostname & setup grub
echo "my-system" > /etc/hostname
passwd
grub-install --target=x86_64-efi --efi-directory=/boot/efi --bootloader-id=GRUB
grub-mkconfig -o /boot/grub/grub.cfg

# enable networkmanager
systemctl enable NetworkManager.service


# Finished!
```