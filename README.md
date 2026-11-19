# 確定境界論（D-Boundary）— LLM Inference Size: Measuring the Size of Work an LLM Can Complete

**Deterministic Boundary Theory (D-Boundary)**

> **English summary:** This repository presents a deterministic, single-run method for measuring an LLM's *inference size*: the size of work it can actually complete. Instead of benchmark scores or success probabilities, the method locates the exact point where a model starts to fail — errors land deterministically on the structurally hardest operation, which means the processing is accurate and only the width is insufficient. Work is then split at that measured failure point. The practical protocol is a halving ladder: hand over work split in half, escalate if it completes, split further (25%, 12.5%) if it does not. Measured on 2026-11-17 (Gemma 4 31B, Qwen3.7-Plus, GPT): with no change to hardware or software (the model), fitting the handover procedure to the measured size raised delivered performance and cut wasted time and tokens. Author: 殿様ソングス (@O0wY4rFtnANmccw).

# LLMの「サイズ」のはかり方 — 完走できる仕事の大きさ

## サイズとは何か

モデルのパラメータ数でも、ベンチマークの点数でもない。
そのLLMが**完走できる仕事の大きさ**（推論サイズ）のこと。
「誰に・何を・どの重さで渡すか」の判断は、このサイズで決める。

## 測り方 — 半減の階段

実測域を事前にテストしない。仕事そのものが測定になる。

1. 最初から、仕事を**半分に分割して**渡す
2. 完走したら、次は**半分にしない物**を渡す
3. 完走できない・無反応・同じ検証を繰り返す（迷路に入る）なら、**即中止**してさらに分割する。**25%** と決めつける。それでも駄目なら **12.5%**
4. 最初の1回で、基準のグループより上か下かが見える

やらない事（無駄と定義する）：

- 積み上げ検証（1+1 のような最小単位から積み上げて測る）
- MAX投げ検証（最大サイズを投げて試す）

## 失敗の読み方

- 間違う場所が「正解が一番難しい操作」に毎回固定されるなら、処理自体は正確で、足りないのは幅だけ。その器は、測った幅の内側では信頼できる
- 境界は欠点ではなく仕様。境界の手前で割れば、実務では壊れない
- 完走できない事・不合格は、そのAIにとって重い。だから最初から完走できる大きさで渡す

## 境界での割り方

- 1つの式を、出来る範囲の段に分割し、その結果を次に渡す形に書き換える。64bit の CPU が 128bit 演算を上下 64+64 に割ってやるのと同じ形で、コストは時間だけ
- 表示変換のような浅い段は、計算させず**手順化**する（数値の付け替えではなく、文字列操作として定義する）

## バージョンチェック

- 測る前に、バージョンを確認して記録する。結果はモデル名ではなく**バージョンに属する**
- バージョンアップで能力が下がっている場合もある

## 実測例（2026-11-17）

- **Gemma 4 31B**：乗算は 7桁×3桁 まで安全。3桁の数の畳み込みは 8項まで。それを超えると、答えが生成値になる（前の答えの数字列を延長する形も観測）。最終計算を分割形に書き換えたら、9桁の中間値でも全段一致した
- **Qwen（Qwen3.7-Plus）**：形（分割・原点ルール・容積の半減）を合わせたら、実務形の計算シートを滑りゼロで完走した。器の実効能力は、運用の仕方で伸びて、信頼性も上がる

## まとめ

- 測るのは点数ではなく、完走できる大きさと、壊れる場所
- 渡し方は、半分から始める階段で決める
- 割る場所は失敗位置から、手順化する段は浅い所から
- バージョンを先に確認する

## リポジトリ構成

- `README.md` — 方法の本体（このファイル）
- `measurement-prompt.md` — サイズを測るプロンプト（投げ包＋キー）。誰が走らせても同じ境界が再現されるか確かめられる
- `tests.md` — 2026-11-17 のテスト記録の要約（シートテスト、Gemma の失敗位置の診断、計算容積テスト）
- `implementation.md` — 実装ノート：分割形・手順化・半減の梯子を実務の受け渡しに適用する形
- `related-work.md` — 類似論文の調査（一次ソース付き）。先行研究はいずれも確率的な境界か外在的な物差しであり、決定的な単回境界を仕様にする形は見つからなかった
- `cm-d-boundary.md` — CM-D-Boundary（Check Max D-Boundary、略称 CMD-B）：大きな器の測り方。スケールが見えない器の計測法（倍＋階乗形の上り、÷√2 の下り）、スケールランクの規定、レベル呼称（+1、+2…）と +3／+4 の予測

## 著者

殿様ソングス（X: @O0wY4rFtnANmccw）
