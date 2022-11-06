# MiyadaikuEngine 1.0

!!! Info
    本エンジンは、オープンソースで開発しています。  
    エンジンソース：[https://github.com/mewmew-tea/MiyadaikuEngine1.0:material-open-in-new:](https://github.com/mewmew-tea/MiyadaikuEngine1.0){:target="_blank"}

## 設計思想

## アーキテクチャ概要

## 特徴とその解説

- [マルチプラットフォームのための工夫](./multiplatform.md)  
- [レンダラについて](./renderer.md)  
- [C#スクリプティングシステム](./scripting.md)  
- [エディタについて](./editor.md)  
- [RuntimeとEditor間のプロセス通信（IPC）](./ipc.md)  
- [ソースコードの品質管理（CI/CD）](./cicd.md)  

## 備考：開発の際参考にしたゲームエンジンなど

ゲームエンジン開発をするためには、先人のノウハウを知っておく必要があります。  
そこで、既存のものについて、エンジンソースを読むなどの調査をし、多くの時間を割いています。

- [Unity](https://unity.com/ja)：全般的なエンジン設計
- [UnrealEngine](https://www.unrealengine.com/ja/)：エンジンソースを含む。レンダラ設計など。
- [Effekseer](https://github.com/effekseer/Effekseer)：開発参加することで得たノウハウ全般、CMakeビルドスクリプトなど
- [GodotEngine](https://github.com/godotengine/godot)：C#スクリプトなど
- [SpartanEngine](https://github.com/PanosK92/SpartanEngine)
- [LumixEngine](https://github.com/nem0/LumixEngine)
- RE Engine（カプコン）：最適なエンジン設計を求めているうちに、近い設計になっていました。それに気づいて以来、過去のCEDECの講演資料などを熟読して参考にしています。
- Cyllista Game Engine（Cygames）：過去のCEDECの講演資料などを熟読して参考にしています。

ほか多数。
