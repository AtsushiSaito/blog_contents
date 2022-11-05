---
title: "AtomからVSCodeに乗り換えた話"
date: 2018-11-15T01:04:50+09:00
categories:
tags:
keywords:
comments: false
showMeta: false
showActions: false
---

## はじめに
最近までatomを使ってコーディングしたりしていました.atom使っていて, どうしようもなく不便ということはないのですが, ちょくちょく気になっていたのが起動の遅さと全体的なもっさり感です. 自分がatomを使うときの環境はmacがメインなのですが, 初回の起動がめちゃ遅いです. 自身のPCの性能が糞な可能性もありますが, とりあえず遅いです.

そんな訳で, いい感じのエディタはないかなって探していたら見つけました.VSCodeと呼ばれるものです.

## VSCodeとは？
> Visual Studio Codeはソースコードエディタである.マイクロソフトにより開発され, Windows, Linux, macOS上で動作する[5].デバッグ, Gitクライアントの統合, シンタックスハイライト, インテリセンス, スニペット, リファクタリングなどの機能を持つ.カスタマイズもでき, 利用者はエディタのテーマやキーボードショートカット等を変更できる.
[Visual Studio Code - Wikipedia](https://ja.wikipedia.org/wiki/Visual_Studio_Code)

## とりあえずインストール
インストールの方法は, [VSCode](https://code.visualstudio.com/download)のダウンロードページに飛んで, 使用している環境に合わせたものをダウンロードすればよいです.
mac版は, ダウンロードボタンを押して, ダウンロードしたzipをファイルを展開すると, 中にアプリケーションファイルが入っているので, macのアプリケーションフォルダにコピーします.
あとは, コピーしたVSCodeのアプリケーションファイルを開けば, VSCodeが立ち上がります.

## プラグインを入れまくる
自分の周りにVSCodeを使っている人がいたので, 教えてもらったプラグインなどを入れていきます.

* [Python](https://marketplace.visualstudio.com/items?itemName=ms-python.python)
* [node.js](https://marketplace.visualstudio.com/items?itemName=leizongmin.node-module-intellisense)
* [Material Icon Theme](https://marketplace.visualstudio.com/items?itemName=PKief.material-icon-theme)
* [GitLens](https://marketplace.visualstudio.com/items?itemName=eamodio.gitlens)
* [ESLint](https://marketplace.visualstudio.com/items?itemName=dbaeumer.vscode-eslint)
* [CMake Tools](https://marketplace.visualstudio.com/items?itemName=vector-of-bool.cmake-tools)
* [C/C++](https://marketplace.visualstudio.com/items?itemName=ms-vscode.cpptools)
* [Bracket Pair Colorizer](https://marketplace.visualstudio.com/items?itemName=CoenraadS.bracket-pair-colorizer)
* [background](https://marketplace.visualstudio.com/items?itemName=shalldie.background)

## 使ってみた感想
まず, atomの欠点として挙げていた起動速度ですが, めちゃ早いです, 一瞬で起動します. 先程のプラグインを全ていれた状態で起動しても, 起動速度に変化はなかったです. 起動後の動作においても, 補完などのプラグインが入っていても動作が遅くなる感じはありませんでした. というかこの補完機能がめちゃ強いです. かなりコーディングのストレスが減らせます.
結論として, VSCodeはかなり快適に動作しており, 現状特に問題が発生していないので, このまま暫く使ってみたいと思います.

以下は, 現在使っている設定です.

```json
{
  "workbench.iconTheme": "material-icon-theme",
  "editor.fontSize": 13,
  "editor.formatOnSave": true,
  "workbench.startupEditor": "newUntitledFile",
  "explorer.confirmDelete": false,
  "workbench.colorTheme": "Monokai Dimmed",
  "git.enableSmartCommit": true,
  "remote.onstartup": true,
  "editor.renderIndentGuides": true,
  "latex-workshop.latex.recipes": [
    {
      "name": "platex",
      "tools": [
        "platex",
        "pbibtex",
        "platex",
        "platex",
        "dvipdfmx"
      ]
    }
  ],
  "latex-workshop.latex.tools": [
    {
      "name": "platex",
      "command": "platex",
      "args": [
        "-synctex=1",
        "-interaction=nonstopmode",
        "-file-line-error",
        "-kanji=utf8",
        "-shell-escape",
        "%DOC%"
      ]
    },
    {
      "name": "dvipdfmx",
      "command": "dvipdfmx",
      "args": [
        "%DOCFILE%.dvi"
      ]
    },
    {
      "name": "pbibtex",
      "command": "pbibtex",
      "args": [
        "-kanji=utf8",
        "%DOCFILE%"
      ]
    }
  ]
}
```