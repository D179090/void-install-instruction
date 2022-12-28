краткая инструкция по установке  void linux 
для начала разделы
мне нужно минимум 3
где:
1.раздел ефи
2.раздел своп
3.раздел корень
# cfdisk /dev/nvme0n1
# mkfs.vfat -F 32 /dev/nvme0n1p1
# mkswap /dev/nvme0n1p2 
# swapon /dev/nvme0n1p2
# mkfs.btrfs /dev/nvme0n1p3
монтируем разделы
# mkdit /mnt/void
# mount /dev/nvme0n1p3
# mkdir /mnt/void/boot/efi
# mount /dev/nvme0n1p1 /mnt/void/boot/efi
базовая установка
выбор репы
# REPO=https://repo-default.voidlinux.org/current
копируем RSA-ключи
# mkdir -p /mnt/var/db/xbps/keys
# cp /var/db/xbps/keys/* /mnt/var/db/xbps/keys/
установка базовой системы
# XBPS_ARCH=x86_64 xbps-install -S -r /mnt/void -R "https://repo-default.voidlinux.org/current" base-system
в случае установки через ROOTFS 
монтируем псевдофайловые системы
# mount --rbind /sys /mnt/sys && mount --make-rslave /mnt/sys
# mount --rbind /dev /mnt/dev && mount --make-rslave /mnt/dev
# mount --rbind /proc /mnt/proc && mount --make-rslave /mnt/proc
копируем resolv.conf
# cp /etc/resolv.conf /mnt/etc/
# PS1='(chroot)
# chroot /mnt/void /bin/bash
(обычная установка)
имя хоста
# echo void > /etc/hostname
локали
# nvim /etc/default/libc-locales
пароль root-пользователя
# passwd
генерируем fstab
# cp /proc/mounts /etc/fstab
(хотя проще написать самому)
ставим загрузчик
# xbps-install -S grub efibootmgr
# grub-install --target=x86_64-efi --efi-directory=/boot/efi --bootloader-id="Void"
# mkdir -p /boot/efi/EFI/boot
# cp /boot/efi/EFI/Void/grubx64.efi /boot/efi/EFI/boot/bootx64.efi
финал
# xbps-reconfigure -fa
ребут



после установки
добавляем пользователя
# useradd --create-home d179090
# passwd d179090
группы пользователя
# usermod -aG wheel audio video d179090
# xbps-install lightdm lightdm-gtk-greeter xfce4 neovim xorg 
# ln -sf /etc/sv/lightdm /var/service
# sv start lightdm 
Вроде всё :)
написал D179090

