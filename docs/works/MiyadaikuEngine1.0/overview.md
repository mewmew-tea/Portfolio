# MiyadaikuEngine 1.0

![](../../images/MiyadaikuEngine1.0.png)

!!! Note
    [昨年度（２０２１年度）も自作ゲームエンジンを開発していました](../MiyadaikuEngine/overview)が、新たに２０２２年夏ごろより開発をはじめました。


## 目標・設計思想

本エンジンのおもな目標は３つです。  

- マルチプラットフォーム対応
- 開発イテレーションの向上
- ソースコードの品質の向上、維持


## アーキテクチャ概要

先述の目標から、エンジン設計を丁寧に時間をかけて行いました。  
エンジンそのものの土台となる、コア部分を重視して開発しています。

![](../../images/Architecture_20221110.svg)

大きな特徴は４つあります。

- ApplicationとEditorの完全分離（別exeファイル）
- ApplicationとEditor間のプロセス間通信(IPC)
- C#スクリプトで記述するゲームロジック
- C#とWPF、MVVMパターンを用いて開発されたエディタ

## 特徴とその解説

- [マルチプラットフォームのための工夫](./multiplatform.md)  
- [レンダラとRHI](./renderer.md)  
- [C#スクリプティングシステム](./scripting.md)  
- [エディタについて](./editor.md)  
- [RuntimeとEditor間のプロセス間通信（IPC）](./ipc.md)  
- [ソースコードの品質管理（CI/CD）](./cicd.md)  


## [今後の開発予定](./loadmap.md)

長期的に開発を続けます。  

---

## 備考：参考にしたもの

CEDECなどの講演をはじめとして、様々な書籍や既存のゲームエンジンなどを参考にしています。  

### 書籍など

数多くありますが、一部を紹介します。  

- [「ゲームエンジンアーキテクチャ 第3版」](https://www.borndigital.co.jp/book/19115.html)  
ゲームエンジン設計の考え方や、必要な機能を知りました。
- [「C++のためのAPIデザイン」](https://www.sbcr.jp/product/4797369151/)  
C++でのライブラリ自作に関する知識を得ました。
- [「.NETのクラスライブラリ設計　改訂新版」](https://bookplus.nikkei.com/atcl/catalog/21/S80040/)  
C#のエディタやスクリプト用のAPI設計と実装の参考にしました。
- [「Effective C# 6.0/7.0」](https://www.shoeisha.co.jp/book/detail/9784798153865)、[More Effective C# 6.0/7.0」](https://www.shoeisha.co.jp/book/detail/9784798153988)  
C#のコードやAPIをよりよくする際の参考にしました。
- [「Direct3D12 ゲームグラフィックス実践ガイド」](https://gihyo.jp/book/2021/978-4-297-12365-9)  
DirectX12などの最新世代グラフィックAPI設計を知りました。
- [「実例で学ぶゲーム3D数学」](https://www.oreilly.co.jp/books/9784873113777/)  
算術ライブラリ自作のために、数学について復習する目的で読みました。

ほか多数。

### ゲームエンジン、ライブラリなど

ゲームエンジン開発をするためには、先人のノウハウを知っておく必要があります。  
そこで、既存のものについて、エンジンソースを読むなどの調査をし、多くの時間を割いています。

- [Unity](https://unity.com/ja)：全般的なエンジン設計
- [UnrealEngine](https://www.unrealengine.com/ja/)：エンジンソースを含む。レンダラ設計など。
- [GodotEngine](https://github.com/godotengine/godot)：C#スクリプトなど
- [SpartanEngine](https://github.com/PanosK92/SpartanEngine)
- [LumixEngine](https://github.com/nem0/LumixEngine)
- RE Engine（カプコン）：最適なエンジン設計を求めているうちに、近い設計になっていました。それに気づいて以来、過去のCEDECの講演資料などを熟読して参考にしています。また、２０２２年９月の「オープンカンファレンス RE:2022」にも自主的に参加し、知識を得ています。
- Cyllista Game Engine（Cygames）：過去のCEDECの講演資料などを熟読して参考にしています。

ほか多数。
