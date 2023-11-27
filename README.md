OpenWRT on the R4S
==================

Get an R4S device running a fresh install of OpenWRT

1. Put image on to SD card

 1.1 format sd card.

    sudo mkfs -t ext4 /dev/mmcblk0

 1.2 dd image.

    wget https://downloads.openwrt.org/releases/23.05.2/targets/rockchip/armv8/openwrt-23.05.2-rockchip-armv8-friendlyarm_nanopi-r4s-ext4-sysupgrade.img.gz
    gunzip openwrt-*-sysupgrade.img.gz
    sudo dd if=openwrt-*-sysupgrade.img of=/dev/mmcblk0

 1.3 expand partition (via cfdisk). Run cfdisk, select the second partition and resize it, write and quit

    sudo cfdisk /dev/mmcblk0

2. Prepare some files to connect

  2.1 /etc/config/network
  2.2 /etc/config/system

3. Boot the R4S

 3.1 expand partition

    # From https://openwrt.org/docs/guide-user/advanced/expand_root#examples
    opkg update
    opkg install parted losetup resize2fs
    sh /etc/uci-defaults/70-rootpt-resize

4. Add packages. Additional config

 4.0. opkg update && opkg install $(cat packages.txt)
 4.1. /etc/config/network
 4.2. /etc/config/firewall
 4.3. /etc/config/ddns
