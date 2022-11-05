---
title: "UnityのParticleSystemでグラフを作成する方法"
date: 2018-07-02T03:14:59+09:00
categories:
tags:
keywords:
comments: false
showMeta: false
showActions: false
---

## Unityでグラフ描画
みなさん, Unityでグラフを書いたことはありますか？
Unityには標準でグラフを表示する機能が備わっていないので, 自分で作るかアセットに頼るしかありません.
しかし, アセットストアにあるグラフ関連のアセットが高いこと...
なので, Unityに標準で備わっている`ParticleSystem`で擬似的にグラフを作成してみました.
グラフの題材にはごく普通の正規分布を使用して, アニメーションは適当に三角関数を使って行いました.

{{< youtube gVxBtm0kypI >}}  
作成したソースコードはGitHubに置いてあるので、興味のある方はぜひご覧ください.
https://github.com/AtsushiSaito/Plot_Normal_Distribution

