OpenWRT on the R4S
==================

Get an R4S device running a fresh install of OpenWRT

1. Put image on to SD card

    1. format sd card.

        sudo mkfs -t ext4 /dev/mmcblk0

    2. dd image.

        wget https://downloads.openwrt.org/releases/23.05.2/targets/rockchip/armv8/openwrt-23.05.2-rockchip-armv8-friendlyarm_nanopi-r4s-ext4-sysupgrade.img.gz
        gunzip $(echo openwrt-*-sysupgrade.img).gz
        sudo dd if=$(echo openwrt-*-sysupgrade.img) of=/dev/mmcblk0

    3. expand partition (via cfdisk). Run cfdisk, select the second partition and resize it, write and quit

        sudo cfdisk /dev/mmcblk0

2. Prepare some files to connect (copy from prev install.. or skip this an manually configure)

        mkdir ./mnt
        sudo mount /dev/mmcblk0p2 ./mnt/
        sudo cp network firewall dhcp ddns ./mnt/etc/config/

3. Boot the R4S

    1. set a password

        passwd

    2. expand partition

        # From https://openwrt.org/docs/guide-user/advanced/expand_root#examples
        opkg update
        opkg install parted losetup resize2fs
        wget -U "" -O expand-root.sh "https://openwrt.org/_export/code/docs/guide-user/advanced/expand_root?codeblock=0"
        . ./expand-root.sh

4. Add packages. Additional config

    1. Alter hostname

        vi /mnt/etc/config/system

    2. Copy your ssh key

        ssh-copy-id -o PubkeyAuthentication=no -o PreferredAuthentications=password root@10.0.0.1

    3. Install packages

        opkg update && opkg install $(cat packages.txt)

5. Mount additional storage

    1. Plug in the USB drive

    2. Add to fstab

        block detect | uci import fstab

    3. Use the LUCI web ui to mount at specific point

