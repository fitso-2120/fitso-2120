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
