---
title: "Ubuntu 16.04にROS Kineticをセットアップする方法"
date: 2018-05-14T06:50:59+09:00
categories:
tags:
keywords:
comments: false
showMeta: false
showActions: false
---

研究でROSを使うため, Ubuntu 16.04にROSをインストールしてみました.

## インストール手順

※ 最近になってKeyが変更になったらしいので更新しました。

### PPAの追加
```
$ sudo sh -c 'echo "deb http://packages.ros.org/ros/ubuntu $(lsb_release -sc) main" > /etc/apt/sources.list.d/ros-latest.list'
$ sudo apt-key adv --keyserver 'hkp://keyserver.ubuntu.com:80' --recv-key C1CF6E31E6BADE8868B172B4F42ED6FBAB17C654
$ sudo apt update
```

### ROSのインストール(kinetic)
```
$ sudo apt install ros-kinetic-desktop-full
```

### 初期設定

```
$ sudo rosdep init
$ rosdep update
```

### 環境変数を追加

#### bashの場合

```
$ echo "source /opt/ros/kinetic/setup.bash" >> .bashrc
```

#### zshの場合

```
$ echo "source /opt/ros/kinetic/setup.zsh" >> .zshrc
```

###  ワークスペースの作成

#### bashの場合

```
$ source ~/.bashrc
$ mkdir -p ~/catkin_ws/src
$ cd ~/catkin_ws/
$ catkin_init_workspace src
$ echo "source ~/catkin_ws/devel/setup.bash" >> ~/.bashrc
$ catkin_make && source ~/catkin_ws/devel/setup.bash
```

#### zshの場合
```
$ source ~/.zshrc
$ mkdir -p ~/catkin_ws/src
$ cd ~/catkin_ws/
$ catkin_init_workspace src
$ echo "source ~/catkin_ws/devel/setup.zsh" >> ~/.zshrc
$ catkin_make && source ~/catkin_ws/devel/setup.zsh
```

以上でセットアップは完了です.