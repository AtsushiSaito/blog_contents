---
title: "Ubuntu 18.04にROS Melodicをセットアップする方法"
date: 2018-06-20T01:18:00+09:00
categories:
tags:
keywords:
comments: false
showMeta: false
showActions: false
---

つい最近, Ubuntu 18.04 LTSがリリースされていたみたいです.
ROSを使う者として, 新しいUbuntuが出たらすぐインストールしたくなるもんです.
Ubuntu 18.04向けのROSはmelodicという名前になったみたいです.
どういう意味なんだろ...

という話はさておき, 早速インストールしてみました.
内容的にはkineticの時と変わりません.

## 試した環境
* Mac (Parallels Desktop 13)
* Ubuntu 18.04 LTS

## インストール手順
※追記
最近になってKeyが変更になったらしいので更新しました。
### PPAの追加
```sh
$ sudo sh -c 'echo "deb http://packages.ros.org/ros/ubuntu $(lsb_release -sc) main" > /etc/apt/sources.list.d/ros-latest.list'
$ sudo apt-key adv --keyserver 'hkp://keyserver.ubuntu.com:80' --recv-key C1CF6E31E6BADE8868B172B4F42ED6FBAB17C654
$ sudo apt update
```

### ROSのインストール(melodic)
```sh
$ sudo apt install ros-melodic-desktop-full
```

GUI無しの場合
```sh
$ sudo apt install ros-melodic-ros-base
```

### 初期設定

最近になってrosdepがデフォルトでインストールされなくなったらしく、手動でインストールする必要があります.

```
sudo apt install python-rosdep

```

```sh
$ sudo rosdep init
$ rosdep update
```

### 環境変数を追加

#### bashの場合

```sh
$ echo "source /opt/ros/melodic/setup.bash" >> .bashrc
```

#### zshの場合

```sh
$ echo "source /opt/ros/melodic/setup.zsh" >> .zshrc
```

###  ワークスペースの作成

#### bashの場合

```sh
$ source ~/.bashrc
$ mkdir -p ~/catkin_ws/src
$ cd ~/catkin_ws/
$ catkin_init_workspace src
$ echo "source ~/catkin_ws/devel/setup.bash" >> ~/.bashrc
$ catkin_make && source ~/catkin_ws/devel/setup.bash
```

#### zshの場合
```sh
$ source ~/.zshrc
$ mkdir -p ~/catkin_ws/src
$ cd ~/catkin_ws/
$ catkin_init_workspace src
$ echo "source ~/catkin_ws/devel/setup.zsh" >> ~/.zshrc
$ catkin_make && source ~/catkin_ws/devel/setup.zsh
```

kineticの部分をmelodicに置き換えるだけでインストールできました.
以上でセットアップは完了です.