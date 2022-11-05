---
title: "ROSに関する備忘録【個人用】"
date: 2018-08-07T19:29:33+09:00
categories:
tags:
keywords:
comments: false
showMeta: false
showActions: false
---

普段からROSを使っていても, たまにしか使わないコマンドや設定あります. 
例えば, パッケージ作成のコマンドだったり, ROSのホスト設定だったり...
とはいえ, 必要になったときに毎回調べるのは面倒なので, 
個人的なROSに関するコマンドなどをメモを記していこうと思います. 

### パッケージの作成

```sh
catkin_create_pkg hogehoge std_msgs rospy roscpp

```
Pythonを使用する場合: `rospy`  
C++を使用する場合: `roscpp`

### ROS MASTERの設定

```sh
export ROS_MASTER_URI=http://localhost:11311
export ROS_HOSTNAME=$(hostname -I)
```