---
title: "PhotoKitの使い方メモ"
date: 2018-08-14T02:42:42+09:00
categories:
tags:
keywords:
comments: false
showMeta: false
showActions: false
---

かなり前に,  iPhone内の写真を使ったアプリを開発しようと思って,  PhotoKitというライブラリを使っていたのですが,  つい最近になって再び使おうとしたら殆ど忘れていたので,  メモを書いていこうと思います.  

※ここで示す例はシミュレータ環境で実行したものとなります.  
## Photokitのインポート
```swift
import Photos
```

## PHAssetCollectionの取得
PHAssetCollectionはiOSのアルバムのオブジェクトみたいなものです.  
このオブジェクトの中にアルバム名やアセットに関する情報が保存されています.  
```swift
let smartAlbumData = PHAssetCollection.fetchAssetCollections(with: .smartAlbum, subtype: .albumRegular, options: nil)

smartAlbumData.enumerateObjects { (collection, index, _) in
    print(collection.localizedTitle!, PHAsset.fetchAssets(in: collection, options: nil).count)
}

【実行結果】 =>
Favorites 0
Recently Deleted 0
Panoramas 0
Camera Roll 5
Slo-mo 0
Screenshots 0
Bursts 0
Videos 0
Selfies 0
Hidden 0
Time-lapse 0
Recently Added 0
Live Photos 0
Long Exposure 0
Animated 0
Portrait 0
```
各アルバムに含まれている写真の枚数を調べようとしたら,  `PHAssetCollection`クラスにそれらしき関数がなかったので,  先程の例のように`PHAssetCollection`を使って`PHAsset`をフェッチして,  その配列の数を数えるようにしたら良い感じに枚数を取得できました.  
```swift
PHAsset.fetchAssets(in: collection, options: nil).count
```
また,  先程の例では`.smartAlbum`の取得を行っていました.  
この`.smartAlbum`というのは,  システム側(iOS)側が作成したアルバムのみを取得するというオプションです.  つまり,  カメラロールやポートレートなどのアルバムが取得できるということになります.  
そのため,  ユーザが作成したアルバムなども取得したい場合は`.album`オプションを使用して,  `PHAssetCollection`をフェッチする必要があります.    
※ 事前に`TestAlbum`というアルバムを作成して,  適当な画像を1枚入れました.  

```swift
albumData = PHAssetCollection.fetchAssetCollections(with: .album, subtype: .any, options: nil)

albumData.enumerateObjects { (collection, index, _) in
    print(collection.localizedTitle!, PHAsset.fetchAssets(in: collection, options: nil).count)
}

【実行結果】 =>
TestAlbum 1
```

