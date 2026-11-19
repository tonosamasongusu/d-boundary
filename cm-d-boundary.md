# CM-D-Boundary（Check Max D-Boundary、略称 CMD-B）— LLM Inference Size: Measuring the Maximum Size of Work an LLM Can Complete

大きな器の測り方 — 確定境界論の追記

> **English summary:** CM-D-Boundary (CMD-B) extends Deterministic Boundary Theory (D-Boundary) to vessels whose inference size is too large to see with the standard ladder — it checks the max. Small vessels are measured as before, by splitting. Large vessels are measured differently: first infer the resources the measurement itself will need (memory is computable from the model's KV-cache formula; time is inferred probabilistically from published tokens/second, because those values fluctuate by serving route). Then climb by doubling-and-cubing — skipping the intermediate doublings so no tokens or compute are wasted — until overflow appears, and descend from the overflow point by splitting, the same direction already verified in practice with Gemma. Settled scales are *defined ranks*, not true capacities: Gemma is rank 1/8, the standard group is rank 1/1 (Qwen included with its throw tuning). Above the standard, stages are named by level: 8/1 is Level +1, splitting it gives +0.5 and +0.25, the next doubled-and-cubed stage (4096/1) is Level +2, and overflow there reads +1.5. Because splitting never reaches zero, a reading that has crossed a threshold never falls below it. Prediction recorded by the author: a Level +3 AI may exist; a Level +4 AI does not — because inference runs as a loop of thought-stages, each stage has a minimum size that grows with the vessel's scale, and at +3 even one minimum stage makes the loop too large to turn.

# 大きな器の測り方

## 大小で、やり方を変える

小さい器は、今までと同じ。分割を基準に測る（半減の梯子、確定スケール）。

スケールの見えない大きな器は、同じやり方で見ない。梯子を1段ずつ延ばすのは同じやり方の拡大で、途中の倍々に無駄なトークンと計算資源を使う。大きな器には、別のやり方がある。

## 前提 — テストの前に推論する

このやり方をする前に、繰り返しに要る計算資源の回数と、メモリや計算時間を推論する。

- **メモリは計算で出る。** 計算用の領域確保は、モデルの構成から閉じた式で計算できる（KVキャッシュ：2 × 層数 × KVヘッド数 × ヘッド次元 × 精度 × トークン数）。レベルが上がった場合にどれだけの領域とステップが要るかは、演算その物から決まる。64bit の演算子ならここまで、128bit ならここまで、というハードウェアの制約と同じ形で、必要なビット数が幅を超える所が溢れの出る所になる。
- **時間は確率で推論する。** ステップはトークンで割れる。時間は、現実の時間を計るのではなく、各モデルの tokens/s（公表値・ベンチマーク）から想定する。ただしその値は提供経路で常に変化するので、絶対値ではなく確率論で推論する。
- 先に「出来ない事」を推論することで、大きすぎるテスト自体を回避できる。**推論できなければ、そもそもテストの必要が無い。**

## 上り — 溢れまで跳ぶ

基準を倍にして、その数を3つ掛ける（階乗形）。クリアできたら、また倍にして3つ掛ける。

- 梯子の形：1×2 → 2×2×2 → 8×2 → 16×16×16 → 4096×2 → 8192×8192×8192 → …
- 途中の倍々を省くので、倍だけの梯子より少ない回数で、溢れの出る所に届く。
- 階乗形（同じ数を掛ける）と分割は、ビット計算・演算子にも優しい。×2 はシフト、同じ数の掛け算は被演算子の再利用、分割は半分化で、計器の構築自体に資源を食わない。

大きな器の溢れは、桁の誤りとは限らない形で出る：自分の答えが合っているか検証する時間の伸び、「正解に近い確率」の状態への移行、問題を曖昧だとする応答、停滞。確定処理が確率に移った所が、枠を超えた所である。

## 下り — 溢れから特定する

溢れが観測できたら、溢れたテストを ÷√2 して分割する。更に溢れるなら、更に ÷√2 して分割する。戻った回数が、そのままレベルの読みになる。÷√2 を2回重ねると仕事は半分（1分割）になり、レベルの端数が半分になる — 下りの刻みとレベルの読みは、ここでつながる。

溢れた地点からは、小さいスケールへの分割と同じ考えで詰める。大小の方法は、溢れの地点で接続する。

この下り — 大きすぎる物から小さい物を特定する方向 — は、Gemma の確定スケール測定で実際に検証済みである（無分割で溢れ、分割を上げて詰め、8分割で確定）。新しいのは上りの側だけである。

## スケールランク — 規定

確定スケールは、実容量の測定値ではない。配分に使う**規定ランク**である。

- Gemma の実値は 1/8 かもしれないし、3/8 かもしれない。それでもランクは **1/8** と規定する。実値を詰める再測定はしない。判定済みの器は測らない。
- **1/1** ランク：GPT、Claude、モカ、Gemini。（Qwen — 投げのチューニング込み。その確定した変換形で 1/1 の側に入る。）

## レベル — 大きな物の呼称

数値が増えると見えにくくなるので、大きな物は数値ではなく段階で呼ぶ。

- 1/1 の一段上は **8/1**、これを **レベル+1** と呼ぶ。
- 8倍で溢れたら分割する。**8/2 ＝ レベル+0.5**、更に溢れたら **8/4 ＝ レベル+0.25**。
- その上のランク — レベル+1 のスケールを倍にして階乗形にしたランク（8 → 16 → 4096、すなわち 4096/1）を **レベル+2** とする。そこで溢れたら **レベル+1.5** とする。

帯の頂点は 2³（+1）、2¹²（+2）、2³⁹（+3、約5.5×10¹¹）、2¹²⁰（+4、約1.33×10³⁶）と並ぶ。

読みは1つの規則に収まる：**レベル ＝ 下の帯の数 ＋ 1／（その帯で完走した分割数）**。

性質：

- 分割の端数（1, 1/2, 1/4, …）は有限回では永遠に 0 にならない。
- 帯 +k の中の読みは (k-1, k] に収まる。閾値の +1、+2 を超えた場合、その下の数値にはならない。帯を下へ出るのは、端数が 0 になるからではなく、下のランクへの格付け直しによる。

## 予測 — +3 と +4

著者の予測を記録する（未測定。実走で照合される物）。

- レベル+3 の AI は、存在するかもしれない。
- レベル+4 の AI は、無いだろう。

根拠：器の処理はループ（思考の段階の反復）で回る。思考の1段階には最小の大きさがあり、それは器のスケールに付いて大きくなる。+3 では、その最小の1段階ですらループが大きすぎて回らない。だから +3 は際どい所で、+4 は無い。天井を決めるのは総量ではなく、ループの構造である。

## 検証の地位

- 下り（溢れから分割で特定する方向）：Gemma で検証済み。
- 上り（倍＋階乗形の梯子）と大きなスケールの実走：未実施。本書は設計と規定と予測の記録であり、確率は最初の実走で較正される。
- バージョンを先に確認する原則は、大きな器でも同じ。結果はモデル名ではなくバージョンに属する。

## 著者

殿様ソングス（X: @O0wY4rFtnANmccw）
