---
title: "【dhcpcd × wpa_supplicant】Raspberry Piで無線LANを設定する方法"
date: 2019-01-10T18:40:36+09:00
categories:
tags:
keywords:
comments: false
showMeta: false
showActions: false
---

## はじめに

皆さんはRaspberry Piで無線LAN設定をするときは、どのように行っていますか？
おそらく`ifupdown`などを使用して、`/etc/network/interfaces`などに設定を記述している方が多いと思います。
`ifupdown`を使用した設定方法は巷にたくさん書いてあるので、ここでは、また別の設定方法を紹介したいと思います。

## なぜこの記事を書いたか？

少し前に、Raspberry Pi 3B+でUbuntu Server 16.04を動作させる記事を書いたとき、`ifupdown`を使った無線LAN設定を行った際、
`wpa-roam`と呼ばれるローミング機能が動作せず、他の方法を模索したのが今回の方法を書くキッカケとなりました。
`wpa-roam`は飛んでいる電波に応じて、自動的に切り替えを行うことができるもので、使用できないと不便です。
なので、他の方法を模索したとき、`dhcpcd`と呼ばれるDHCPのクライアントを使用することで、従来通りに無線LANの接続に加えて、
`wpa-roam`と同等の機能を使うことが確認できたので、記事に書いた次第です。
それでは、設定方法について紹介をしていきます。

## 必要なソフトウェアのインストール
```sh
sudo apt install dhcpcd5 wpasupplicant wireless-tools
```

/etc/network/interfaceの内容を全部コメントアウト

```sh
sudo vim /etc/network/interfaces
```

↑で開いたらコメントアウトする。

## wpa_supplicant.confの設定
```sh
sudo vim /etc/wpa_supplicant/wpa_supplicant.conf
```

ssidとパスワードは各自変更してください。
```sh
country=JP
ctrl_interface=DIR=/var/run/wpa_supplicant GROUP=netdev
update_config=1
network={
    ssid="hogelan"
    psk="hogepass"
}
```

## wpa_supplicantの自動起動サービスを作成

```sh
sudo vim /etc/systemd/system/multi-user.target.wants/wpa_autostart.service
```
以下を入力
```sh
[Unit]
Description = wpa_supplicant auto start service.

[Service]
ExecStart = /sbin/wpa_supplicant -Dwext -iwlan0 -c/etc/wpa_supplicant/wpa_supplicant.conf
Restart = always
Type = simple

[Install]
WantedBy = multi-user.target
```

サービスを有効にします。
```sh
sudo systemctl daemon-reload
sudo systemctl enable wpa_autostart.service
sudo systemctl start wpa_autostart.service
sudo systemctl status wpa_autostart.service
```
## dhcpcdの設定

```sh
sudo vim /etc/dhcpcd.conf
```

```sh
# A sample configuration for dhcpcd.
# See dhcpcd.conf(5) for details.

# Allow users of this group to interact with dhcpcd via the control socket.
#controlgroup wheel

# Inform the DHCP server of our hostname for DDNS.
hostname

# Use the hardware address of the interface for the Client ID.
#clientid
# or
# Use the same DUID + IAID as set in DHCPv6 for DHCPv4 ClientID as per RFC4361.
# Some non-RFC compliant DHCP servers do not reply with this set.
# In this case, comment out duid and enable clientid above.
duid

# Persist interface configuration when dhcpcd exits.
persistent

# Rapid commit support.
# Safe to enable by default because it requires the equivalent option set
# on the server to actually work.
option rapid_commit

# A list of options to request from the DHCP server.
option domain_name_servers, domain_name, domain_search, host_name
option classless_static_routes
# Most distributions have NTP support.
option ntp_servers
# Respect the network MTU. This is applied to DHCP routes.
option interface_mtu

# A ServerID is required by RFC2131.
require dhcp_server_identifier

# Generate Stable Private IPv6 Addresses instead of hardware based ones
slaac private

# 以下を追記
interface eth0
interface wlan0

## 固定にする場合は以下も追記

## 固定にしたいネットワークのSSID
ssid hogelan


## 固定アドレスを指定
## xxxを適宜変更
static ip_address=192.168.22.xxx/24
## ルータのアドレスを設定
static routers=192.168.22.1
## ルータのアドレスと同じのでOK
static domain_name_servers=192.168.22.1
```

以上の設定が完了したら再起動します。

## 無線LANの接続確認

無線LANに接続できているか確認します。
```sh
$ iwconfig
```

```sh
eth0      no wireless extensions.

lo        no wireless extensions.

wlan0     IEEE 802.11  ESSID:"hogelan"  
          Mode:Managed  Frequency:2.437 GHz  Access Point: xxxxxxxxxxxxxxx   
          Bit Rate=72.2 Mb/s   Tx-Power=31 dBm   
          Retry short limit:7   RTS thr:off   Fragment thr:off
          Power Management:on
          Link Quality=70/70  Signal level=-33 dBm  
          Rx invalid nwid:0  Rx invalid crypt:0  Rx invalid frag:0
          Tx excessive retries:9  Invalid misc:0   Missed beacon:0
```

ssidが表示されれば接続できています。

## IPアドレスの確認

```sh
$ ip addr
```

```sh
3: wlan0: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc pfifo_fast state UP group default qlen 1000
    link/ether xxxxxxxxxxxxxxx brd ff:ff:ff:ff:ff:ff
    inet 192.168.22.221/24 brd 192.168.22.255 scope global wlan0
       valid_lft forever preferred_lft forever
    inet6 xxxxxxxxxxxxxxxxxxxxxxxxxxxxx/64 scope link 
       valid_lft forever preferred_lft forever
```

接続できていますね。
これで設定は完了です。不明な点がありましたら、コメントを頂けると助かります。