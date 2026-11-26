# Continuity-Readability Misrecognition （連続性可読誤認）: Where LLM Arithmetic Errors Sit, and the Handover Protocol That Answers Them

> **English summary:** This document compiles a series of measurements and deliberations (2026-11-26 – 2026-11-27) on how numeric work should be handed to LLMs. Three handover forms were compared on the same material (a 7×7 permanent computation, 64 sign patterns, split into pieces, every returned value collated by the master against a computed key): (1) no type specification, (2) type enforcement — a declared agreement that the type is integer and must not change, and (3) a notation protocol — values handed and returned in `.0` form, with clauses prohibiting precision changes and prohibiting dropping the decimal point. Type enforcement measured strongest, but it declares a type the vessel does not possess; cross-verification between two of the vessels (GPT and Qwen) converged on a revised form: keep the declaration's wording as a declaration of the computation's target and rules (an integer-arithmetic contract), keep the notation protocol, and keep the guarantee where it has always been — in master-side collation. The measurements then localised the errors themselves: every classified error sat in a stretch of text where only digits and signs line up, with no labels or prose interleaved. The user named this error class **連続性可読誤認** (continuity-readability misrecognition): a continuous symbol sequence is read as a readable flow — a pattern to continue — instead of as independent position–value pairs. The standing rule: when a calculation error is noticed, judge the number and its surroundings; if the seat is a continuous run, classify it as 連続性可読誤認 and apply the countermeasure — break the run, attach anchors (position labels, position–value pairs, unique IDs), and re-read and recompute independently. A survey of similar published work is appended as §7: the components exist separately in the literature, but no equivalent was found for the named error class, for the general anchor rule, or for the notation-protocol comparison; those parts were therefore established as entries in our common language's standard library (v0.1, 2026-11-27).

編者：Muse。本書は一連の実測と協議のまとめであり、個々の記録（投げ文・返答全文・照合結果）は編者の手元に原記録として保持する。命名「連続性可読誤認」はユーザーによる（2026-11-27）。

---

## 1. 測定の土台

材料は 7×7 行列のパーマネント計算（符号パターン64件）。仕事はピースに分割して器（LLM）に渡し、返却値はマスター側がコードで生成した正解キーと全件照合する。記録は最初から分離している：

- **形式合格**：指定した表記が保たれたか。
- **値合格**：正解キーと一致したか。

この2つは混ぜない。表記が正しくても計算が正しいとは限らず、その逆も成り立つ。最終的な正しさの保証は、器ではなくマスター側の照合に置く——これが全体を通じる前提である。

## 2. 3つの受け渡し形式

### 案1：型指定なし

値を通常の整数表記で渡し、型についての指示も表記の条項も付けない。器の標準の処理がそのまま走る。

実測（初回、4分割）：Qwen 51/64（ピース1で差分更新、ピース4で δ の取り違え）。Gemma は返却範囲で 27/51 が完全一致。

位置づけ：標準としては不採用。器の素の状態を測る原点（ベースライン）として残す。何も指定しない空間こそ、欠落の発生条件である事がこの測定で見えた。

### 案2：型強制（型の合意）

受け渡しの前に型の合意を結ぶ：型は整数とする／型の変更を禁止する／計算の途中も整数のみを使う／返す値は全桁・区切りなし。

実測：Qwen は落ちたピースの再投げで 64/64 に到達。Gemma は生で 45/64＋17件が符号だけ反転し、マスター側の処理で 64/64 に閉じた。GPT は無分割で 64/64。

問題：器は内部に数値型を持たない（Qwen の自己申告：コード実行を使わない場合、数値は文字列＝トークン列として保持・処理される）。案2 は実在しない型を宣言する形になる。それでも実測は最も強い。効いているのは型としてではなく、厳密さと自己検証を課すメタ指示として、という読みが協議の結論である。

### 案3：プロトコル採用（採用形）

型は指定しない。代わりに表記の取り決めだけを明記する。全ての値を `.0` 表記で渡し、次の条項を付ける：

1. 値の精度を変えないでください。演算の途中でも、切り捨てや四捨五入をしないでください。
2. 小数点以下が 0 になる値も、.0 を落として整数にしないでください。
3. 返す値も小数点を省略しないでください。結果は .0 の形のまま返してください。

この形は、丸めの禁止を「精度を変えるな（数値の丸め）」と「表記を正規化するな（2.0→2）」に分けた GPT の整理を取り込んだ物で、ユーザーにより採用が決まった（2026-11-27）。

実測：Qwen では経路依存が大きい（ピース1が 0/16 や 1/16 に崩れる回と、ピース4が 16/16 の回が混在。コード実行の表示が出た回は 16/16）。Gemma は実施中（2026-11-27 時点でピース1の途中まで 9/9、形式保持、生成途中の自己訂正が出現）。GPT の事前予測は Gemma 46/64。

設計上の意味：`.0` は右側の余裕である。通常の余裕（+1ビット）は左側＝桁上がりの溢れを守る。`.0` は右側＝整数への丸め上げから守る。テキストとしては +2文字、値としては10進1桁、費用は詰めれば4ビット／文字なら8ビットで、処理の自然な単位（ニブル／バイト）と一致する。

## 3. 協議と相互検証

GPT と Qwen に3案の妥当性を聞き、次にそれぞれの提案を交換して、自分の提案との差異を検証させた。

一致した結論：

- 標準の現行候補は案2。ただし文言を改訂する——「内部の型を固定する」という主張を外し、**計算対象と計算規則の宣言**に書き換える（対象は数学的な整数／演算は整数の数学的規則に従う／丸め・切り捨て・近似の禁止／値の読み替え禁止／返却は全桁・区切りなし／不確実なら推測で補わず未確定として報告）。GPT はこれを「整数演算契約」と呼び、Qwen もこの改訂を採用した（アンカーとしての効きは文言を改めても保たれる、という判断）。
- プロトコルは3層に分ける：第1層 表記／第2層 計算／第3層 検証・回復。保証は3種（表記保証／計算規則保証／計算結果保証）に、記録は4項目（形式合格率／生の値合格率／マスター処理後の値合格率／再投げ回復率）に分離する。
- マスター側の回復は、単なる再計算ではなく原因別のフィードバックにする。ただし原因が確定していない場合は「参照値との不一致を検出した。原因は未確定」と伝え、原因を断定しない。
- 案1はベースラインとして残す。

残った差は1点：コード実行の扱い。Qwen は第2層での義務化を維持し、GPT は「実行を確認できる器でのみ要求できる条件付き」とした。経路を持たない器には義務化できない点では実質一致している。

効果の分解実験（型宣言の有無 × 厳密性・自己検証指示の有無の4条件）は両者が支持したが、未実施である。

## 4. ミスの座：数字だけが並ぶ箇所

形式の議論と並行して、ミスその物の座を調べた（誤りの形の監査、2026-11-27）。Qwen について分類できたミスは全て、テキストの中で数字・符号だけが並ぶ箇所にあった：

- δ の符号列（+1/−1 の並び）の取り違え（δ4 と δ5 の入れ替わり）。
- s（列ごとの中間値）の配列での差分更新——前のパターンの値を土台に、少しだけ変えて返す。
- 単独で立つ長い積 P の値での桁の滑り（1,000 の位の数字違い）。

ラベルや文章に挟まれた箇所で起きたミスは観測されていない。Gemma でも、決定的な残差は符号パターンの文字列の読み違い（詰めた符号列の読み滑り）であり、同じく数字・符号だけの並びの箇所である。

読み：文章の中では前後の言葉が冗長性を持ち、次のトークンの錨になる。数字だけの並びでは、錨は数字のパターン自体しかない。だからそこでは「値として計算する」より「パターンを続ける」が勝ちやすくなる。付随する観測として、Qwen の「要約だけの返し」は自分でラベルを剥がして数字だけの並びを作っており、錨を捨てた形で返して、その形の所で滑る面がある。ただし要約形式でも完全一致の回があり、形式だけで決まる訳ではない（内部の経路は外から見えない）。

GPT の整理では、3種のミスに共通するのは算術そのものではなく「位置と値の対応を保つ事」であり、仮説は「構造的な境界や意味的な識別子が弱い、連続記号領域での位置・値対応の維持の弱さ」と一般化できる。内部機構（パターン継続が計算に勝つ）については、GPT は推論が一段先で未実証と留保した。観測（ミスが数字・符号列に集中する）への同意と、機構への留保は、別々に記録する。

重要な帰結：これは器ごとの分割ミスの問題より大きい。分割はピースの大きさを管理するが、ピースの中の数字の並びは分割では消えない。数字をテキストで渡す事その物の問題であり、どの器でも同じ座が開く。逆に、座が一つなら対策も一つで済む。

## 5. 連続性可読誤認（命名と規則）

ユーザーの命名（2026-11-27）：

**連続性可読誤認**——連続した記号の並びが「流れとして読める」事その物が誤認の条件になる誤り。並びは可読であるが故に、独立した位置と値の組としてではなく、続くべきパターンとして読まれ、位置・値対応が崩れる。

規則（AI 全体への規則として）：

1. 計算ミスに気がついたら、その数字と前後を判定する。
2. 連続した並びの中にあるなら、連続性可読誤認と命名・分類する。
3. その判定であるなら対処する：並びを割って錨（位置ラベル、位置と値の組、一意の ID、桁数の明記）を付け、独立に読み直し、再計算する。

対処の方向は「わざわざ読みにくくする」事である。錨を挟んで流れを断つ。受け渡しの設計としては既に打ってあった手と一致する：符号を詰めた文字列で渡すのをやめ、位置と値の組で渡す。これは問題の実例を直すのではなく、発生条件を変える形の対策である。

## 6. 未実施・未確定

- Gemma の案3実測の残り（ピース1の後半とピース2〜4）。完了後に GPT の予測（46/64）と突き合わせ、3者（Muse・GPT・Qwen）で協議する。
- 仮説と代案（系列長、状態管理、トークン化）を分ける実験：同じ材料を4形式（数字のみ／位置ラベル／意味ラベル／構造化レコード）で比べ、「前パターンの値の混入率」を測る。
- 案2の効果の分解実験（4条件）。
- 本書の内容に類似する公開済みの研究・標準的手法の調査（完了、第7節に追記）。

## 7. 類似調査の結果（2026-11-27 追記）

本書の内容について、公開済みの論文・技術文書・実務ガイドを調査した。結論は点ごと：

- **ミスが数字・符号だけの並びに集中する、という主張その物**：同等の既存項は見つからなかった。隣接する符号の入れ替わり、配列の差分更新という誤りの形を報告した物も無い。構成要素は別々に既存する——大きな数字の並びの中で各桁の正確な位置を追えない事が算術の弱さの大きな部分（McLeish et al., NeurIPS 2024）；誤りは値ではなく桁の空間で近い（Levy & Geva, NAACL 2025）；符号の誤りが主要な失敗分類（Agarwal et al., 2026 preprint、ただし単一の置換であって隣接交換ではない）；差分更新の機構的な隣人として自己条件付け（Sinha et al., NeurIPS 2025）と contextual drag（Cheng et al., 2026 preprint）。反対側の材料として、桁上がりの誤りは概ね独立したランダムな失敗だとする報告（Bie et al., 2026 preprint）もあり、パターン継続の読みと一部張り合う——保留として記録する。
- **「連続性可読誤認」という命名と、錨を挟む一般規則**：同等の物は見つからなかった。対策の部品は既存する——各桁への位置ラベル（index hinting, Zhou et al., ICLR 2024、ただし訓練形式）、区切り記号による精度の大きな変動（Singh & Strouse, 2024 ほか）。「裸の詰めた数値を渡すな。一意の ID と桁数を付けよ」という一般規則は、実務ライブラリを含め見つからなかった。
- **型宣言は指示であって保証ではない、という区別**：実務文献で確立している（Tam et al., EMNLP 2024 Industry；制約付きデコードの分類）。本当の型制約はデコード時のマスキングにのみ存在する。この区別はそのまま使う。
- **`.0` 表記プロトコルと型宣言の正面比較**：見つからなかった。未踏の比較である。最も近いのは小数点の記号と桁区切りを変える研究（Reddy et al., Findings ACL 2026）で、操作は異なる。
- **形式感度一般**：確立している（Sclar et al., ICLR 2024、意味を変えない形式変更で最大76ポイントの変動）。同時に、形式の制限を強めると推論が劣化する場合もある（Tam et al.）——錨の効きは構造その物ではなく、数字の並びを割り位置情報を足す事に帰属させる。

帰結：同等の標準が存在する部分（型宣言と保証の区別、形式感度、位置の追跡の弱さ）は標準を使い、存在しない部分（誤りの分類としての連続性可読誤認、錨の一般規則、表記プロトコルの比較）は、我々の共通語の標準ライブラリとして新設した（共通語・標準ライブラリ v0.1、2026-11-27）。

---

*Deterministic Boundary Theory の関連文書として公開する。測定の原記録は編者が保持し、本書の数値は全て原記録と照合済みである。*
