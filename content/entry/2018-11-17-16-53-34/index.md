---
title: "UICollectionViewCellの画面上での座標を取得する方法"
date: 2018-11-17T16:53:34+09:00
categories:
tags:
keywords:
comments: false
showMeta: false
showActions: false
---

## はじめに
諸事情でUICollectionViewCellの画面上の位置が取得する必要があったので, やり方を調べてみました.

## 取得方法

取得するには, UICollectionViewをスクロールした際のオフセットと, UICollectionViewからのCellの位置の差を取ればいいので, 以下のような感じで取得してあげます.

```swift
extension UICollectionView{
    func nowPosition(cell: UICollectionViewCell) -> CGRect{
        let point = CGPoint(x: cell.frame.origin.x - self.contentOffset.x, y: cell.frame.origin.y - self.contentOffset.y)
        let size = cell.bounds.size
        return CGRect(x: point.x, y: point.y, width: size.width, height: size.height)
    }
}
```

使い方としては, UICollectionViewの拡張メソッドとして定義したので, こんな感じで使ってあげてください.
```swift
let cell: UICollectionViewCell = collectionView.cellForItem(at: indexPath)!
            print(self.MainCollection.nowPosition(cell: cell))
```

カスタムセルを使用している場合は, 以下のようにキャストして使います.
```swift
let cell: UICollectionViewCell = collectionView.cellForItem(at: indexPath) as! MainCollectionCell
            print(self.MainCollection.nowPosition(cell: cell))
```
