---
title: "RaspberryPi 3にscikit-learnを入れた話"
date: 2018-05-14T06:02:54+09:00
categories:
tags:
keywords:
comments: false
showMeta: false
showActions: false
---

scikit-learnと言えば機械学習のライブラリで有名ですね.
今回は, 研究室で使うロボットに入れるのが目的なので,
RaspberryPi 3を対象に話を進めていきます.

※ 2019年12月1日更新
* sudo pip installでインストールすると、場合によってはPythonの環境が壊れる場合があるので、`--user`でインストールしたほうが良さそうです。

## aptを使った場合
apt経由だと少し古いバージョンの0.17しかインストールできませんでした.
今回の目的は, 0.18以降で実装された教師なし学習の変分ベイズ混合ガウス分布(クラスタリング)が使いたいため,
これでは目的を達成できません.

## 追記
apt経由でも最新のバージョンが取得できるようになったみたいです.

## pipでインストール
pip経由でインストールすることで, 
最新版のscikit-learnがインストールできました.

まず, 先に依存関係にある`scipy`をインストールします.
`scipy`はpip経由でもインストールできますが,
コンパイルの時間が長いので,
apt経由でインストールします.
```
$ sudo apt install python-scipy
```

最後に, 以下のようにpipでイン

```
$ pip install scikit-learn -U --user

```

## メモ

* pipコマンド自体がエラーで実行できない問題があった。
```
  File "/usr/lib/python2.7/dist-packages/OpenSSL/SSL.py", line 118, in <module>
    SSL_ST_INIT = _lib.SSL_ST_INIT
AttributeError: 'module' object has no attribute 'SSL_ST_INIT'
```
* 解決方法
    * `sudo python -m easy_install --upgrade pyOpenSSL`

## 参考文献
 * https://stackoverflow.com/questions/45188413/python-pip-install-is-failing-with-attributeerror-module-object-has-no-att



apt経由でも最新版がインストールできるようになるといいなぁ(希望)