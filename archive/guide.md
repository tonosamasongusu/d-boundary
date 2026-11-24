# 案内 — このリポジトリの読み方

> **English summary:** This repository records a method for measuring an LLM's *inference size* — the size of work it can actually complete — in two papers: Deterministic Boundary Theory (D-Boundary, in the README) for measuring smaller vessels by splitting, and CM-D-Boundary (CMD-B) for vessels too large to see that way, measured by doubling-and-cubing. Every number in the papers comes from the calculation sheet; check against the calculation, not the prose. The measurement prompt is self-contained so anyone can re-run the measurement and reproduce the boundary. Status: the splitting/descent direction is verified in practice (Gemma); the large-vessel ascent is design, definitions and a recorded prediction, calibrated at its first run.

## これは何か

LLM の「推論サイズ」（その器が完走できる仕事の大きさ）を測る方法の記録。論文2本と、再現のための道具一式が入っている。

- **第1論文：確定境界論（D-Boundary）** — 小さい器を分割で測る方法。`README.md` が本体。
- **第2論文：CM-D-Boundary（CMD-B）** — スケールの見えない大きな器を、倍＋階乗形の梯子で測る方法。`cm-d-boundary.md`。
- 論文中の数値は、全部 **計算書**（`cm-d-boundary-calculation-sheet.md`）の規則と計算から出ている。検証は本文の散文ではなく、この計算と突き合わせる事。

## 読む順番

1. `README.md` — 方法の本体（D-Boundary：半減の梯子、失敗の読み方、バージョンチェック）
2. `cm-d-boundary.md` — 大きな器の測り方（CMD-B：前提の推論、上りと下り、スケールランクとレベルの規定、+3／+4 の予測）
3. `cm-d-boundary-calculation-sheet.md` — 数値の根拠（梯子、帯の頂点、レベルの読み、下りの最大回数）
4. `measurement-prompt.md` — サイズを測るプロンプト（投げ包＋キー）。手元の LLM で同じ境界が再現するか試せる
5. `tests.md` — テスト記録の要約／`implementation.md` — 実装ノート／`related-work.md` — 類似論文の調査（一次ソース付き）

## 検証の地位 — 何が済んでいて、何が未実施か

- **分割で測る方向（下り）：実測・検証済み。** 確定スケールは、Gemma 4 31B が 8分割（ランク 1/8）、Qwen（Qwen3.7-Plus）が 2分割（投げのチューニング込みでランク 1/1 の側）、GPT・Claude・Gemini は無分割（ランク 1/1）。
- **大きな器の上り（倍＋階乗形）：未実施。** 設計と規定と予測の記録であり、確率は最初の実走で較正される。
- **予測として記録済み：** レベル+3 の AI は存在するかもしれない。レベル+4 の AI は無いだろう（根拠は CMD-B 論文の「予測」の節）。

## 再現について

- `measurement-prompt.md` の投げ包は自己完結している。渡して完結する形で、誰が走らせても同じ境界が出るかどうかで確かめる設計。
- 測る前にバージョンを確認して記録する事。結果はモデル名ではなく、バージョンに属する。

## 著者

殿様ソングス（X: @O0wY4rFtnANmccw）
