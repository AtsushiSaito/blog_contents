---
title: "Eigenの使い方メモ"
date: 2018-08-12T18:09:49+09:00
categories:
tags:
keywords:
comments: false
showMeta: false
showActions: false
---

EigenはC++で使用できる行列演算ライブラリです.   
Eigenを使うと行列やベクトルが簡単に扱えて,  ある行列の逆行列や行列式などを簡単に求めることができます. 
便利なものはどんどん使いたいので,  メモ程度に使い方を書いていきます. 

## 行列やベクトルの定義

Eigenでは,  4次元までの正方行列はMatrix4dのように定義できます. 
```cpp
Eigen::Matrix2d A;  // 2x2のdouble型の行列
Eigen::Matrix4d B;  // 4x4のdouble型の行列
```
```cpp
Eigen::Vector2d a;  // 1x2のdouble型のベクトル
Eigen::Vector4d b;  // 1x4のdouble型のベクトル
```
4次元以降の場合や長方行列の場合は以下のように定義します. 
```cpp
Eigen::Matrix<double, 10, 10> A;  // 10x10のdouble型の行列
Eigen::Matrix<double, 5, 10> B;  // 5x10のdouble型の行列
```
要素数が不明な行列を定義する場合は以下のようにします. 
```cpp
Eigen::MatrixXd A;  //不明な要素数の行列
Eigen::VectorXd a;  //不明な要素数のベクトル
```

## 初期化
カンマ区切りで初期化する場合は,  以下のようにします. 
```cpp
Eigen::Matrix2d A;
A << 1,2,3,4;

実行結果 =>
1 2
3 4
```
配列やvectorから初期化したい場合は,  Mapを使って初期化します. 
```cpp
Eigen::Matrix2d A;
vector<double> a;
for(int i = 1;i<5;i++){
    a.push_back((double)i);
}
A = Eigen::Map<Eigen::Matrix<double, 2, 2, Eigen::RowMajor> >(a.data());

実行結果 =>
1 2
3 4
```
`Eigen::RowMajor`は行優先で行列を初期化するというもので,  これを設定しない場合は以下のようになります. 
```cpp
Eigen::Matrix2d A;
vector<double> a;
for(int i = 1;i<5;i++){
    a.push_back((double)i);
}
A = Eigen::Map<Eigen::Matrix<double, 2, 2> >(a.data());

実行結果 =>
1 3
2 4
```
このように,  配列の中身が列ごとに初期化されるようになっています. 
最初に書いたカンマ区切りと同様に初期化したい場合は,  `Eigen::RowMajor`を付けるといいと思います. 

ちなみに,  `Eigen::MatrixXd`で定義した行列にも,  Mapを用いた初期化を行えます. 
```cpp
Eigen::MatrixXd A;
vector<double> a;
for(int i = 1;i<5;i++){
    a.push_back((double)i);
}
A = Eigen::Map<Eigen::Matrix<double, 2, 2, Eigen::RowMajor> >(a.data());

実行結果 =>
1 2
3 4
```