---
title: "RaspberryPi3で複数のWi-Fiを自動で切り替える方法"
date: 2018-05-21T17:42:08+09:00
categories:
tags:
keywords:
comments: false
showMeta: false
showActions: false
---

## interfaces設定
`/etc/network/interfaces`を書き換えて設定を行います.

適当なテキストエディタで編集します.

`$ sudo vim /etc/network/interfaces`

以下のように追記 & 編集します.

```sh
auto lo
iface lo inet loopback

#------eth0----------
allow-hotplug eth0
iface eth0 inet dhcp
#--------------------

#-----wlan0----------
allow-hotplug wlan0
auto wlan0
iface wlan0 inet manual
wpa-roam /etc/wpa_supplicant/wlan0_wpa_supplicant.conf
wireless-power off

# 自宅の設定(適宜変更)
iface HOME inet static
address 192.168.3.180
netmask 255.255.255.0
gateway 192.168.3.1
dns-nameservers 192.168.3.1

# 研究室(適宜変更)
iface LAB inet static
address 192.168.22.130
netmask 255.255.255.0
gateway 192.168.22.1
dns-nameservers 192.168.22.1
#-------------------
```
次にWi-FiのSSID・Passwordの設定をします。
```sh
sudo vim /etc/wpa_supplicant/wlan0_wpa_supplicant.conf
```

以下のように追記 & 編集します.

```sh
update_config=1

network={
    ssid="研究室のSSID"
    psk="暗号化されたパスワード"
    id_str="LAB"
}

network={
    ssid="自宅のSSID"
    psk="暗号化されたパスワード"
    id_str="HOME"
}
```

暗号化されたパスワードを生成するには, 以下のコマンドを使います.

```sh
$ wpa_passphrase SSID パスワード
```

これで設定は終了です.
設定を反映するには,　システムの再起動または, wlan0を再起動が必要です.
以下のどちらかを実行してください.

### システムの再起動をする場合
```sh
$ sudo reboot
```

### wlan0の再起動する場合
```sh
# wlan0が起動してる場合は終了
$ sudo ifdown wlan0

# wlan0を起動
$ sudo ifup wlan0
```

最後に動作しているか確認します.
```sh
$ ip addr

```

IPアドレスなどが表示されていれば設定は完了です.

お疲れ様でした.