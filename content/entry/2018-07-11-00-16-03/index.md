---
title: "RaspberryPi 3B+でUbuntu 16.04を起動させる方法"
date: 2018-07-11T00:16:03+09:00
categories:
tags:
keywords:
comments: false
showMeta: false
showActions: false
---

前の記事では, RaspberryPi 3B+でUbuntu18.04を動作させる方法を書きました.   
Ubuntu 18.04が動くだけでも十分ですが, 普段ROSを使うときはKineticを使用しているので, Ubuntu 16.04が使えると色々捗ります. 

そんな訳で, 以下に動作させるまでの手順を記します. 
## 注意事項
こちらである程度は不具合がないことを確認していますが, 未確認の不具合があるかもしれません. 
そのため, 不具合が出ても大丈夫な環境で試してください. 

## 現在確認できている問題点

* ifupdownで無線LANを設定する際, wpa-roamを使用するとwlan0が起動できない. 

## イメージデータの公開
この記事の手順で作成したイメージデータを公開しておきます. 

UserName: `ubuntu`  
Password: `ubuntu`

#### アップグレードまで設定済み
[ubuntu-16.04.4-preinstalled-server-armhf+raspi2+upgraded-20180805.img.xz](https://github.com/AtsushiSaito/Ubuntu16.04_for_RaspberryPi/releases/download/v0.1/ubuntu-16.04.4-preinstalled-server-armhf+raspi2+upgraded-20180805.img.xz)  
File Size : 360MB  
MD5: 38c3e846351fae9032ea10b5710f7214
#### アップグレード＆ROS Kinetic
[ubuntu-16.04.4-preinstalled-server-armhf+raspi2+upgraded+ros-preinstalled-20180805.img.xz](https://github.com/AtsushiSaito/Ubuntu16.04_for_RaspberryPi/releases/download/v0.1/ubuntu-16.04.4-preinstalled-server-armhf+raspi2+upgraded+ros-preinstalled-20180805.img.xz)  
File Size : 688MB  
MD5: 4b163e6c3537ae56e3a04ef882530212
#### アップグレード＆ROS Kinetic＆カーネルコンパイル済み
[ubuntu-16.04.4-preinstalled-server-armhf+raspi2+upgraded+ros-preinstalled+kernel-compiled-20180805.img.xz](https://github.com/AtsushiSaito/Ubuntu16.04_for_RaspberryPi/releases/download/v0.1/ubuntu-16.04.4-preinstalled-server-armhf+raspi2+upgraded+ros-preinstalled+kernel-compiled-20180805.img.xz)  
File Size: 1.02GB  
MD5: 7a83369cd9d981906b3ed8312457dadd
#### ライセンス

* Ubuntu Server 16.04.4
    * https://wiki.ubuntu.com/ARM/RaspberryPi
    * GPLv3 License
* ROS Kinetic
    * http://www.ros.org/
    * BSD License
* Wi-Fi Firmware
    * RPi-Distro/firmware-nonfree
    * https://github.com/RPi-Distro/firmware-nonfree/blob/master/LICENCE.broadcom_bcm43xx

## 準備
今回使うUbuntu 16.04のイメージはARM/RaspberryPiのWikiから取得しました.   
[ubuntu-16.04.4-preinstalled-server-armhf+raspi2.img.xz](http://cdimage.ubuntu.com/ubuntu/releases/16.04/release/ubuntu-16.04.4-preinstalled-server-armhf+raspi2.img.xz)

ddやEtcherなどでイメージを書き込んでください. 

次の項目からUbuntu Desktopで作業をしていきます. 
## miciroSDのマウント
私の環境ではmicroSDを接続すると, 自動で`/media/atsushi/`にマウントされました.   
※ atsushiはユーザ名です. 

## パーティションの中身を編集
[https://github.com/raspberrypi/firmware](https://github.com/raspberrypi/firmware)

上記のリポジトリをクローンします. 
```sh
cd ~/
git clone --depth 1 https://github.com/raspberrypi/firmware.git
```
`boot`と`module`を各ディレクトリにコピーします. 
```sh
cd ~/firmware
sudo cp -r modules/* /media/atsushi/cloudimg-rootfs/lib/modules/
sudo cp -r boot/* /media/atsushi/system-boot
```
次に, config.txtを編集します. 
```sh
vim /media/atsushi/system-boot/config.txt
```
`kernel=uboot.bin`の記述を消します. 
```sh
kernel=uboot.bin
device_tree_address=0x02000000
```
```sh
device_tree_address=0x02000000
```

これでRaspberryPi 3B+で起動できるようになりました.   
しかし, まだ問題があるので更に変更を加えていきます. 

## flash-kerneの書き換え
```sh
sudo vim /usr/share/flash-kernel/db/all.db
```
ファイルの末尾に追記します. 
```sh
Machine: Raspberry Pi 3 Model B Rev 1.2
Kernel-Flavors: rpi2 rpi2-rpfv
DTB-Id: bcm2710-rpi-3-b.dtb
Boot-Kernel-Path: kernel7.img
Boot-DTB-Path: bcm2710-rpi-3-b.dtb
Boot-Device: /dev/mmcblk0p1

Machine: Raspberry Pi 3 Model B Plus Rev 1.3
Kernel-Flavors: rpi2 rpi2-rpfv
DTB-Id: bcm2710-rpi-3-b-plus.dtb
Boot-Kernel-Path: kernel7.img
Boot-DTB-Path: bcm2710-rpi-3-b-plus.dtb
Boot-Device: /dev/mmcblk0p1
```

## /bootの中身を掃除
```sh
cd /boot && sudo rm -rf v* r* i* g* c* a* S* firmware.bak/
```

## 無線LANの設定

内蔵の無線LAN(wlan0)を使用する為には, 関連したファイルを更新する必要があります. 

```sh
cd ~/
mkdir wifi-firmware
cd wifi-firmware

wget https://github.com/RPi-Distro/firmware-nonfree/raw/master/brcm/brcmfmac43455-sdio.bin
wget https://github.com/RPi-Distro/firmware-nonfree/raw/master/brcm/brcmfmac43455-sdio.clm_blob
wget https://github.com/RPi-Distro/firmware-nonfree/raw/master/brcm/brcmfmac43455-sdio.txt 
sudo cp *sdio* /lib/firmware/brcm/
cd ../
```

再起動するとwlan0(無線LAN)が動作するようになります. 

## ホストの名前解決
```sh
sudo sh -c 'echo 127.0.1.1 $(hostname) >> /etc/hosts'
```

## アップデート
```sh
sudo apt update && sudo apt upgrade
```

## cloud-initの削除
cloud-initはUbuntuの初回起動時にユーザー作成やら色々やってくれるのですが, 
初回起動以降は不要になるので, 消しておきます. 
```sh
sudo apt purge cloud-init
```

## A start job is running for raise network interfaces
以下を/etc/network/interfacesに追記する. 
```sh
allow-hotplug eth0
iface eth0 inet dhcp
``` 
## パスワード変更
```sh
sudo passwd ubuntu
```

## 無線LANの設定
NetworkManagerを使う.   
[http://akkiesoft.hatenablog.jp/entry/20150625/1435240904](http://akkiesoft.hatenablog.jp/entry/20150625/1435240904)
```sh
sudo apt-get install network-manager
```
/etc/network/interfacesの中身を空にする. 

ここを参考に設定する. 
[https://qiita.com/kure/items/c12f01789c902012c230](https://qiita.com/kure/items/c12f01789c902012c230)


## SSH安定化

```sh
sudo vim /etc/ssh/ssh_config
```
以下を追記
```sh
IPQoS cs0 cs0
```

```sh
sudo systemctl restart ssh
```
## 不具合？
wpa-roamを使うと正常にifupができない. 

# おまけ

## ROS Kineticのインストール
[https://github.com/ryuichiueda/ros_setup_scripts_Ubuntu16.04_server](https://github.com/ryuichiueda/ros_setup_scripts_Ubuntu16.04_server)

## Raspberry PI Mouse関連
デバイスドライバをインストールするには, カーネルビルドをする必要があります.   
※カーネルビルドを行う前に, `sudo mkdir -p /boot/overlays/`を行ってください.   
[https://github.com/ryuichiueda/raspberry_pi_kernel_build_scripts](https://github.com/ryuichiueda/raspberry_pi_kernel_build_scripts)

デバイスドライバをインストールします.   
`./build_install.raspbian.bash`を実行してください.   
[https://github.com/rt-net/RaspberryPiMouse](https://github.com/rt-net/RaspberryPiMouse)

起動時にデバイスドライバを自動で読み込む設定  
[https://github.com/ryuichiueda/pimouse_setup](https://github.com/ryuichiueda/pimouse_setup)

## イメージ公開の前処理
```sh
sudo apt install kpartx
```
マウントします. 
```sh
sudo kpartx -a ~/ubuntu.img
sudo mount /dev/mapper/loop0p2 /mnt/
```
以下のコマンドを実行. 

```sh
cd /mnt/home/ubuntu
rm .sudo_as_admin_successful
rm .bash_history && touch .bash_history
rm .viminfo
rm .lesshst
sudo find /mnt/var/log/ -type f -name \* -exec cp -f /dev/null {} \;
```
空き領域をゼロ埋めする. 
```sh
sudo dd if=/dev/zero of=/mnt/dummy bs=4096
sudo rm /mnt/dummy
```
アンマウントしてxz圧縮する. 
```sh
cd ~/
sudo umount /mnt
sudo kpartx -d /dev/loop0
xz -vk ubuntu.img
```

## 参考文献

* [https://wiki.ubuntu.com/ARM/RaspberryPi](https://wiki.ubuntu.com/ARM/RaspberryPi)
* [https://memoteki.net/archives/1101](https://memoteki.net/archives/1101)