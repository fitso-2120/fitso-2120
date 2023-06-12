- 👋 Hi, I’m @fitso-2120. I'm not good at English.<br/>
  I am an engineer who mainly designs and develops web-based systems and builds infrastructure.
- 👀 I’m interested in Rust-lang.<br/>
  For now, we plan to list programs that use Rust-lang on this site.<br/>
- 🌱 I'm changing my old program to Rust-lang.
  <br/>
  When porting to different platforms, how to handle graphics is a problem.<br/>
  Rust-lang does not have a graphics API by default.<br/>
  There is no program that strongly requires conversation, so<br/>
  I decided to output directly to a file as PNG etc.<br/>
  <br/>
  It is a policy to write things that require conversation with nodejs + (WASM + Rust-lang).<br/>
  <br/>
  Process the data with Rust-lang and handle the conversation part and data display with HTML5. It should be easy to test because it can be independent only with Rust-lang.

<!---
- 💞️ I’m looking to collaborate on ...
- 📫 How to reach me ...
--->

---

In Japanese.<br/>
日本語では、もっぱら能書きを中心に。

## 繰り返しで出会った壁

```rust
for i in min..max {...}
```
ってハマります。もちろん
```c
for(int i=min; i<max; i++) {...}
```
という記述なら、*min* から開始して、*max-1* まで繰り返すとわかるのですが<br/>
*in* 表記だと *max* まで繰り返しに含むように見えませんか？

`c`風の文法廃して、繰り返しを *in* 表記にまとめてしまった影響でしょうか。とてもとても紛らわしいです。

### 知識不足
```rust
for i in min..max {...}
```
について
```rust
for i in min..=max {...}
```
という記法が存在していて、こちらは正しく`max`まで繰り返す。
**=** をつけて区別する形でおさまった・・・のかな。
<!---
fitso-2120/fitso-2120 is a ✨ special ✨ repository because its `README.md` (this file) appears on your GitHub profile.
You can click the Preview link to take a look at your changes.
--->

## 使用するAPIについて
いくつかツールなどを作る際に使用するAPIについて、同じ目的で違うものを使っていたので統一しようかなと（といっても、方針はともかく実施はぼちぼちと）

<details>
  <summary>コマンドライン引数の取り込みについて</summary>
[clap](https://crates.io/crates/clap) を用いる

```toml
[dependencies]
...
clap = { version = "*", features = ["derive"] } # 割と頻繁に更新されているので、不安ならバージョンを明記したほうが良いかも
...
```

としておけば、
```rust
use clap::{Parser, ValueEnum};

#[derive(Clone, Debug, PartialEq, ValueEnum)]
pub enum DumpType {
    Char,
    Line,
}

#[derive(Clone, Debug, Parser, Default)]
#[command(author, version, about, long_about = None)]
pub struct Args {
    /// Morse code speed in `wpm` units
    #[arg(short, long, default_value = "25")]
    pub wpm: u8,

...

/// power for audio volume
    #[arg(long, default_value = "2.5")]
    pub power: f32,

    /// Dump message line by per char or per line
    #[arg(short, long)]
    pub dump: Option<DumpType>,

    // group 設定では、グループ内の項目は、すべて指定ないか、たかだか一つの項目を指定することはができる。
    // group の一つに必須条件をつければ、グループの一つの指定が必須となる
    /// The message directly as a command line argument
    #[arg(name = "TEXT", group("text"), required = true)]
    pub text: Option<String>,

    /// Read messages from standard input
    #[arg(short, long, group("text"))]
    pub pipe: bool,
}
```

といった感じで、変数名の頭１文字、変数名そのものをデフォルトで short/long 引数として定義、同時に省略時の値も定義できる。
更には、単項の必須項目や複数の項目のうち一つを指定みたいなことも、「定義」として記述できるので非常にありがたい。
ヘルプやバージョン情報もデフォルトで自動作成される。

あとは、入力範囲やファイル名の有効・無効、存在チェックを追加するだけ。
</details>
<details>
  <summary>GUI について</summary>
`Rust` に限った話ではないが、言語仕様に GUI やグラフィック関連の機能が標準では含まれていない。
自由に選べるといえるが、普段はこれを使うみたいな方針がないと迷いに迷う。

マルチプラットフォームでWEBでも使えるだけでも結構な種類がある。
そこで、 最初の選択肢に egui と そのフレームワーク [eframe](https://crates.io/crates/eframe) とする。もちろん、ウィジェットや機能に不足があるなら個別に別のものを使用する。

egui / eframe の特徴は、イベントドリブンではないこと。
画面構築しながらボタン押下時のコードが入るのでコードが分離されず安心感がある。
反面、表示項目や入力項目が大量になってくると記述が大変になってくるのだけれど、そんな複雑な画面ってそんなにない！ということで、安心感を求めて採用！
</details>
<details>
  <summary>Windowのみのアプリについて</summary>
Rust でコンパイルされた実行モジュールを起動した場合、必ずコマンドプロンプトの画面がでる。ログとかを画面に表示してみたい場合は便利なのだけど、
GUIとして起動するなら、不要。
この場合、main() を記述しているファイルの先頭に

```rust
#![windows_subsystem = "windows"]
```
を書いておけば、コマンドプロンプトの画面は起動されない。
</details>
