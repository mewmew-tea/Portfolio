# C#スクリプティングシステム

MiydaikuEngine 1.0は、C#スクリプティングに対応しています。 

このようにコンポーネントのスクリプトを記述することができます。
```csharp title="弾を発射するPlayerのスクリプト"
public class PlayerController : MonoBehaviour
{
    public float speed = 0.05f; // 弾の速さ
    public int interval = 40;   // 再度発射できるようになるまでのフレーム数
    private int counter = 0;    // 直近に発射してからの経過フレーム数
    private List<GameObject> bullets;   // シーンに存在する弾のゲームオブジェクト
    
    // 初期化。Runtime（C++）が自動で呼び出してくれる。
    private void Init()
    {
        // find bullet gameobjects
        GameObject[] gameObjects = gameObject.GetAll();
        bullets = new List<GameObject>();
        foreach (GameObject obj in gameObjects)
        {
            if (obj.GetComponent<BulletController>() != null)
            {
                bullets.Add(obj);
            }
        }
    }

    // 更新。Runtime（C++）が自動で呼び出してくれる。
    private void Update()
    {
        Vector3 pos;
        pos = transform.LocalPosition;

        // キーボード入力＆移動
        if (Runtime.GetKey('W')) { pos.y += speed; }
        if (Runtime.GetKey('S')) { pos.y -= speed; }
        if (Runtime.GetKey('A')) { pos.x -= speed; }
        if (Runtime.GetKey('D')) { pos.x += speed; }
        // 座標をTransformにセット
        transform.LocalPosition = pos;

        // Shot
        if (Runtime.GetKey('P')) { Shot(); }
        ++counter;
    }

    // 無効な状態の弾を検索し、発射する
    private void Shot()
    {            
        // むやみな連射防止
        if (counter < interval) {return;}
        foreach (GameObject bullet in bullets){
            BulletController controller = (BulletController)bullet.GetComponent<BulletController>();
            if (controller == null) { continue; }
            if (!bullet.Enabled){
                controller.Shot(transform.Position);
                counter = 0;
                return;
            }
        }
    }
}
```

Unityに近い感覚で、ゲームのスクリプトを記述することができます。  

このスクリプティングシステムの大きな特徴は、  
”**C++側に埋め込まれている**”ことです。  

C#とC++の連携手段としてもっとも有名であるものは、  
「C#アプリケーション(.NET)に、C++で記述されたdllを埋め込むこと（P/Invoke）」です。  
しかし、そもそも.NETは一部のプラットフォームにしか対応していません。  
（例えば、.NETはPlaystationやXboxなどのゲーム機には非対応です。）  

この手段では本エンジンの開発目標である、マルチプラットフォーム対応が不可能でした。  
  
そこで、「Mono ([https://www.mono-project.com/:material-open-in-new:](https://www.mono-project.com/){:target="_blank"}) 」を使っています。  
C#を動作させるVM（Virtual Machine/仮想マシン）そのものをNativeRuntime（C++）側へ埋め込むことで、C++側からスクリプトを管理します。  

!!! note
    Monoは、クロスプラットフォームで動作する.NET Framework互換のフレームワークです。


## エンジン標準算術ライブラリ

Runtime(C++)と挙動を合わせるために、算術ライブラリは自作しています。  
また、これらのライブラリは書籍（Effective C#や.NETのクラスライブラリ設計など）を読んで、より良いライブラリとなるように工夫をしました。
例えば、Vector3などは、IEquatableやIFormattableを実装していて、APIとして運用しやすくしています。  

## 型情報とインスタンスの管理

ゲームオブジェクトやコンポーネント、スクリプトなどはすべてC++側で管理されます。  

## 実行までの流れ

C#スクリプトは、実行前にIL（中間言語）に変換されます。  
実行時、ILを読み込んで実行時（JIT）コンパイルされることで実行されます。
実行するのは、Runtimeに埋め込まれたMonoのVM（Virtual Machine/仮想マシン）です。

![](../../images/CSScriptSystem_Flow.svg)


## スクリプト（コンポーネント）が呼び出される仕組み

コンポーネントの基底クラスには、UpdateやInitなどの関数は定義していません。  
つまり、関数のオーバーライドや仮想関数は使っていません。  
  
![](../../images/CSScriptSystem_EventFunction.svg)

NativeRuntime（C++）は、C#スクリプトを読み込んだ際、monoのAPIを用いてコンポーネントの型情報を取得しています。  
その際に「Updateメソッドが存在するか？」を確認しています。  
存在するときのみ、呼び出し対象のリストに登録をしておき、そこから呼び出しています。  
なにもしない関数を呼び出すことを防いでいます。  


## ImGui（エンジンユーザー用）

本エンジンでは、ユーザーがスムーズにデバッグ等をできるようdear ImGuiを組み込んでいます。  
ユーザーはC#スクリプトで、以下のサンプルソースのようにImGuiを表示することができます。

```C# title="C#スクリプトからImGuiウィンドウを表示"
private Vector3 pos = new Vector3(0,0,0);
private void ImGuiUpdate()
{
    if (ImGui.Begin("C# Window"))
    {
        ImGui.Text("This is called from C# script.");
        ImGui.Text("Pos (Vector3) : " + pos);
    }
    ImGui.End();
}
```


<figure markdown>
<figcaption>実行結果</figcaption>
![](../../images/miyadaiku_userImGui.png)
</figure>


## （準備中）ホットリロード機能

現在準備中です。
参考資料として、過去に実験で実現したときの動画を添付します。  

<iframe width="560" height="315" src="https://www.youtube.com/embed/8zmDZixBNWM" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>
