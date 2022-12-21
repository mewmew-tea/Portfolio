# がっぽり！爆走人力車



<iframe width="560" height="315" src="https://www.youtube.com/embed/9TtBH1gOD1E" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>

<table border="1">
<tr><td>ジャンル</td><td>人力車ドライビングアクション</td></tr>
<tr><td>開発期間</td><td>２か月</td></tr>
<tr><td>開発人数</td><td>５人</td></tr>
<tr><td>開発環境</td><td>C#, Unity</td></tr>
<tr><td>プラットフォーム</td><td> Xbox One, Series X|S（<a href=https://www.xbox.com/ja-JP/games/store/44gm44gj44g944kk77yb54ig6lww5lq65yqb6luk/9MXH1BRMFZX0 target="_blank" rel="noopener noreferrer">ストアページ</a>）、WindowsPC（
<a href=https://kobedenshigame.itch.io/jinrikisya target="_blank" rel="noopener noreferrer">itch.ioにて配信中</a>）</td></tr>
<tr><td>担当箇所</td><td>企画、リードプログラマ、人力車（プレイヤー）の挙動基礎設計＆実装、バージョン管理、マルチプラットフォームでの開発管理/パブリッシング</td></tr>
<tr><td>備考</td><td>東京ゲームショウ2022 神戸電子専門学校ブースにて展示</td></tr>
</table>

人力車の車夫として、爆走しながら次々と客をお届けするゲーム。  
企画やリードプログラマなどを担当。  
２か月間という短い期間のなか、５人のプログラマのみで開発。  
Unityで開発し、Xbox Series X|SとWindowsPCでリリース。  

シームレスで広いフィールドを、短期間で作成するために工夫をしました。  

## Unityエディタ拡張

１５０人を超える客を配置するために、エディタを拡張しました。  
当初は「乗車位置」と「降車位置」の２つだけを可視化していましたが、

## 最適化

規模が大きくてシームレスなマップなので、フレームレートの低下が大きな問題となりました。    

プロファイリングの結果、マップ上の建物が特に重いことが分かりました。  

本作は大きくジャンプしてマップを俯瞰することがあるので、遠くも描画する必要があります。  
ですが、開発期間２か月で、プログラマ５人なので、LODモデルを一つ一つ丁寧に用意することは現実的ではありませんでした。  

そこで、一定以上離れた距離にある家は、共通の簡易な3Dモデルとして描画することで対策しました。  

しかし、またもや問題が発生しました。  
共通の簡易3Dモデルとして描画する際、  
拡大縮小を決めるためにバウンディングボックスの情報を取得する負荷が高く、処理落ちしていました。  

これは、バウンディング情報を事前計算しておくことで対策しました。  
エディタ拡張を行い、バウンディング情報を一括で計算、出力するツールを作成しました。  
実行時は、事前計算情報を参照するだけなので、処理負荷を大きく削減できます。

これで、大幅な軽量化を実現しました。


## Xbox版とPC版の同時開発




## 複数のコントローラ種に対応したキーガイドGUI

直近に操作したコントローラの種類を識別して、メニュー上のキーガイドを自動で切り替えるようにしています。  

例えば、  
キーボード＆マウスを使っている間は「Esc：戻る」と表示されます。  
Xboxコントローラを操作し始めると、「B：戻る」と表示されます。  
DualShock（PS4）コントローラを操作し始めると、「×：戻る」と表示されます。  

これらの表示が、実行中自動で切り替わります。  

ちなみに、私が開発に関わったゲームパッド対応のPCゲームには、すべてこの仕組みを実装しています。  

コントローラ種別の識別については、Qiita記事にまとめています。
[Unityでコントローラーの種別を識別する(InputSystem) - Qiita](https://qiita.com/mewmew_tea/items/7d4df683df490b03d9a4)

