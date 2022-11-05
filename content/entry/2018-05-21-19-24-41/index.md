---
title: "RaspberryPiとCGIを使ってブラウザ上でPythonを実行する方法"
date: 2018-05-21T19:24:41+09:00
categories:
tags:
keywords:
comments: false
showMeta: false
showActions: false
---

## はじめに
以前,  JavaScriptを使ってWebアプリを開発したことがあったのですが, 
処理自体はブラウザ側で行うものでした.
そのため, サーバ側でスクリプトを実行したい時は不向きです.

サーバ側でプログラムを実行するには. CGI(Common Gateway Interface)を使うことで解決できます.
CGIは, サーバ側で実行できるものであれば何でも実行できます. (Python, シェルコマンドやら...)

今回は, CGIを用いてPythonのプログラムを実行して, Webブラウザ上に表示してみます.


## 作業ディレクトリを作成
実際にCGIのファイルなどを配置するディレクトリを作成します.
今回はPythonの簡易Webサーバを使うので, 好きなディレクトリに配置してください.

```sh
$ mkdir cgi-test
$ cd cgi-test
```

次に, CGIのスクリプトを配置するディレクトリを作成します.

```sh
$ mkdir cgi-bin
$ cd cgi-bin
```

## CGIのスクリプトを書く

手始めに, Hello Worldを表示するスクリプトを書いてみます.

適当なエディタでファイルを作成 & 編集します.

```sh
$ vim hello.cgi
```

以下の内容を追加します.

```python
#!/usr/bin/env python
# -*- coding: utf-8 -*-

print "Content-type: text/html\n"
print "Hello! World!"
```
書いたスクリプトに実行権限を与えます.

```sh
$ chmod +x hello.cgi
```

## Pythonで安易のWebサーバを起動

Pythonには簡易のWebサーバ機能が備わっているため, 利用してみようと思います.
今回はCGIの機能を使いたいので, `CGIHTTPServer`を使用します.
特にプログラムを書かずに, 作業ディレクトリのルートフォルダで以下のコマンドを実行することで動作します.

```sh
python -m CGIHTTPServer
```

起動したら, PC側のブラウザを開いて, 以下のアドレスにアクセスします.

```sh
http://RaspberryPiのIPアドレス:8000/cgi-bin/hello.cgi

# 例) http://192.168.1.123:8000/cgi-bin/hello.cgi
```

次回は, これの発展版を紹介したいと思います.