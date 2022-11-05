---
title: "VSCodeで日本語TeXの自動コンパイル環境を構築する方法"
date: 2018-12-05T19:32:20+09:00
categories:
tags:
keywords:
comments: false
showMeta: false
showActions: false
---

論文を書いたり卒論を書くときは, TeXを使うことが多いと思います.
以前まではtexファイルをコンパイルするためにMakefileを書いておいて, 保存してはmakeするなどの手順を踏んだりしていました.
今回は, VSCodeに拡張プラグインを入れて, ファイルを保存したら自動でコンパイルできるように設定してみました.

## プラグインのインストール
VSCodeを立ち上げて, `LaTeX Workshop`プラグインをインストールします.
このプラグインは, VSCodeでTeXのシンタックスハイライトを行ったり, 保存時に自動でコンパイルをすることができます.
## ユーザ設定を変更

ユーザ設定に以下の設定を追記します.
```json
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
```

## 自動コンパイルを試してみる
VSCodeで適当なtexファイルを開いて, 編集します.
プラグインの設定や, ユーザ設定が完了していれば保存するだけで自動でコンパイルが行われます.

## おまけ: Latexmkを使用する場合

### VSCodeのsettings.jsonに追記

```json
"latex-workshop.latex.recipes": [
        {
            "name": "latexmk",
            "tools": [
                "latexmk",
                "latexmk_clean"
            ]
        }
    ],
    "latex-workshop.latex.tools": [
        {
            "name": "latexmk",
            "command": "latexmk",
            "args": [
                "%DOC%"
            ]
        },
        {
            "name": "latexmk_clean",
            "command": "latexmk",
            "args": [
                "-c"
            ]
        }
    ]
```
### .latexmkrcを作成

```
vim ~/.latexmkrc
```

以下をコピペ

```perl
#!/usr/bin/env perl
$latex            = 'platex -synctex=1 -halt-on-error';
$bibtex           = 'pbibtex -kanji=utf8';
$dvipdf           = 'dvipdfmx %O -o %D %S';
$max_repeat       = 5;
$pdf_mode         = 3;
$pvc_view_file_via_temporary = 0;
$pdf_previewer = 'open -a /Applications/Skim.app';

$latex = "find . -type f -name '*.tex' -print0 | xargs -0 sed -i '' -e 's/、/, /g' -e 's/。/. /g'; platex -synctex=1 -halt-on-error";
```

## おまけ
macだけの現象なのか, 自動フォーマットが機能しない問題にぶち当たりました.
以下は解決策です.

[ここ](https://www.jianshu.com/p/b23835d06a27)のサイトを参考に以下を実行
```sh
sudo cpan Unicode::GCString
sudo cpan App::cpanminus
sudo cpan YAML::Tiny
sudo perl -MCPAN -e 'install "File::HomeDir"'
```

```sh
sudo cpan Log::Log4perl
sudo cpan Log::Dispatch
```