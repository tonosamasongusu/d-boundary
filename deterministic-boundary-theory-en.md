# Deterministic Boundary Theory (D-Boundary), Fourth Edition — LLM Inference Size: Measuring the Size of Work an LLM Can Complete

**Deterministic Boundary Theory (D-Boundary), Fourth Edition**

> **English summary:** This fourth edition consolidates Deterministic Boundary Theory (D-Boundary) into one document. Part 1 presents the theory: an LLM's *inference size* — the size of work it can actually complete — is measured deterministically in a single run, by splitting work on a halving ladder and reading where failure deterministically lands; the boundary is a specification, not a defect. Part 2 (CM-D-Boundary) extends the method to vessels too large to see that way, using a doubling-and-cubing ladder, defined scale ranks, and level names, with a recorded prediction that a Level +3 AI may exist and a Level +4 AI does not. Part 3 states Operator-Unit Splitting: inference can ultimately be split down to individual operators, the floor of splitting, at which all vessels are exact and vessel differences vanish. Part 4 gives implementation notes; Part 5 collects the test records, including a 2026-11-24 verification on a complex problem (a permanent computation with counting and judgment layers) that confirmed the boundary behaviour end to end. Part 6, new in this edition, states the distributed control of split processing: a master holds the split codes and performs integration, checksum and judgment, while slaves only execute their piece and report — pieces are fully independent, carry their evidence with them, and never need to know what came before. Runs in 2026-11-25 showed that the degradation previously read as a vessel's "coarseness" under fine splitting was produced by the throwing method itself, and that a piece's lower bound is the completeness of its bundled evidence, not its size. Annexes carry the calculation sheet (the ground for every number), the measurement prompt, and a survey of related work. Author: Shoji Miura (finathlon).

## About the Fourth Edition (Editor: Muse)

This fourth edition incorporates "the distributed control of split processing," settled on 2026-11-25, into the documents of the third edition and earlier.

- **First edition (2026-11-17):** The main text of Deterministic Boundary Theory (D-Boundary), together with the guide, the measurement prompt, the test records, the implementation notes, and a survey of related work.
- **Second edition (2026-11-19):** Added CM-D-Boundary (how to measure large vessels) and the standard calculation sheet. Errors caught in external verification (GPT and Qwen, twice each, and Claude's collation of the calculation sheet) have been corrected.
- **Third edition (2026-11-24):** Added the verification record on a complex problem (an appendix to the test records) and Operator-Unit Splitting, and consolidated the whole.
- **Fourth edition (2026-11-25):** Added Part 6, "The Distributed Control of Split Processing." It also includes a correction to the attribution of scale readings (a note to Part 2) and a refinement of the lower bound of a piece (Part 6).

The status of verification is unchanged, as stated at the end of each part: the direction of measuring by splitting (the descent) is measured and verified; the ascent for large vessels (doubling + factorial form) is a record of design, definitions, and prediction, and has not been carried out. The form of Part 6 was verified by a re-test of Qwen at 2-, 4-, and 8-split (with comparison against the old form) and by GPT's collation review (conditional pass; double diagnosis established). 16- and 32-split were supported as design possibilities, but whether the same quality is maintained is unconfirmed (not carried out). The incorporation into the fourth edition itself was verified by the same procedure: it received GPT's collation (conditional pass; the conditions are satisfied in the current text) and Qwen's split collation (new form; the recorded facts agree), and Muse passed it by double diagnosis. Qwen's unconfirmed and partially matching items were attributed to two points — the bundled scope of the verification throw package and the precision of the wording (how the reading of A2 is divided; a note on the scope of the backing for the lower bound) — and the latter was reflected into the main text before publication.

### Editorial Changes (a Ledger of Omissions and Moves)

The main text through the third edition has not been rewritten. The fourth edition made only the following arrangements.

1. The title and English summary of each document were dropped and unified into this book's title and summary.
2. The old README's "Repository structure" section was dropped, as it is replaced by the structure of this book.
3. The author lines were unified at the end.
4. Operator-Unit Splitting, which had been recorded within the test records, was made independent as Part 3 (it is also left, in its original text, on the test-records side).
5. Fourth-edition additions: Part 6 was newly established, and a note correcting the attribution was attached to the scale ranks of Part 2. The words "commander / servant" in conclusion 4 of the Part 5 appendix remain as recorded at the time; from Part 6 onward the vocabulary is unified as "master / slave" (the author's determination, 2026-11-25).

---

# Part 1: Deterministic Boundary Theory

# How to Measure an LLM's "Size" — The Size of Work It Can Run to Completion

## What Size Means

Not the number of a model's parameters, and not a benchmark score.
It is the size of work that the LLM can **run to completion** (its inference size).
The judgment of "to whom, what, and at what weight to hand over" is decided by this size.

## How to Measure — The Halving Ladder

Do not test the measurement range in advance. The work itself becomes the measurement.

1. From the start, hand over the work **split in half**
2. If it runs to completion, next hand over the work **not split in half**
3. If it cannot run to completion, does not respond, or repeats the same verification (enters a maze), **stop immediately** and split further. Fix it at **25%**. If that still fails, **12.5%**
4. In the very first run, you can see whether it is above or below the baseline group

Things not to do (defined as waste):

- Build-up verification (measuring by building up from the smallest unit, such as 1+1)
- Maximum-throw verification (throwing the maximum size to test it)

## How to Read Failure

- If the place of error is fixed, every time, at "the operation where the correct answer is hardest," the processing itself is accurate and only the width is lacking. That vessel can be trusted inside the width measured.
- The boundary is not a defect but a specification. If you split on the near side of the boundary, it does not break in practice.
- Being unable to run to completion, or failing, is heavy for that AI. So hand over, from the start, a size it can run to completion.

## How to Split at the Boundary

- Rewrite a single formula into stages within the range it can do, in a form that hands the result to the next stage. The same form as a 64-bit CPU splitting a 128-bit operation into upper and lower 64+64; the cost is only time.
- Shallow stages, such as display conversion, are not computed but **made into procedure** (defined as string operations, not as a re-assignment of values).

## Version Check

- Before measuring, confirm the version and record it. Results belong not to the model name but to the **version**.
- There are also cases where an upgrade has lowered the capability.

## Measured Examples (2026-11-17)

- **Gemma 4 31B:** Multiplication is safe up to 7 digits × 3 digits. Convolution of 3-digit numbers up to 8 terms. Beyond that, the answer becomes a generated value (a form extending the digit string of the previous answer was also observed). When the final computation was rewritten into split form, all stages agreed even with 9-digit intermediate values.
- **Qwen (Qwen3.7-Plus):** When the form was matched (splitting, origin rule, halving of volume), it ran a practical-form calculation sheet to completion with zero slips. A vessel's effective capability grows with how it is operated, and its reliability rises as well.

## Summary

- What is measured is not a score, but the size it can run to completion and the place where it breaks.
- The handover is decided by a ladder that starts from half.
- Where to split comes from the failure position; which stages to make into procedure, from the shallow places.
- Confirm the version first.

---

# Part 2: How to Measure Large Vessels — CM-D-Boundary (CMD-B)

## Change the Method by Size

Small vessels are as before. Measure them on the basis of splitting (the halving ladder, the determined scale).

A large vessel whose scale cannot be seen is not viewed the same way. Extending the ladder one step at a time is an enlargement of the same method, and uses wasteful tokens and computing resources on the doublings in between. For large vessels, there is another method.

## Premise — Infer Before Testing

Before using this method, infer the number of computing resources the repetition will require, and the memory and computation time.

- **Memory comes out by calculation.** Allocation of the working area can be calculated in a closed formula from the model's structure (KV cache: 2 × number of layers × number of KV heads × head dimension × precision × number of tokens). How much area and how many steps a higher level requires is determined by the operation itself. In the same form as a hardware constraint — with a 64-bit operator, up to here; with 128-bit, up to here — the place where the required number of bits exceeds the width is the place where overflow appears.
- **Time is inferred by probability.** Steps divide by tokens. Time is not measured as real time, but assumed from each model's tokens/s (published values, benchmarks). But because those values always change with the delivery route, infer not by absolute value but by probability theory.
- By inferring "what cannot be done" first, the oversized test itself can be avoided. **If you cannot infer it, there is no need to test in the first place.**

## The Ascent — Jump Until Overflow

Double the baseline, and multiply that number three times (factorial form). If it clears, double again and multiply three times.

- The form of the ladder: 1×2 → 2×2×2 → 8×2 → 16×16×16 → 4096×2 → 8192×8192×8192 → …
- The stages themselves are generated by f(x) = (2x)³: 1 → 8 → 4,096 → 2³⁹ → 2¹²⁰. The ×2 values appearing in the ladder's sequence (2, 16, 8,192) are intermediate values within a stage, not measurement stages. Judgment descends by the cubed value.
- Because the doublings in between are omitted, it reaches the place where overflow appears in fewer runs than a ladder of doublings alone.
- The factorial form (multiplying the same number) and splitting are kind to bit computation and operators as well. ×2 is a shift, multiplication of the same number reuses the operand, splitting is halving, and the construction of the measuring instrument itself consumes no resources.

The overflow of a large vessel does not necessarily appear in the form of digit errors: an extension of the time spent verifying whether its own answer is right, a shift into a state of "probability of being close to the correct answer," a response treating the problem as ambiguous, stagnation. The place where deterministic processing has shifted into probability is the place beyond the frame.

## The Descent — Pinpoint from Overflow

Once overflow is observed, divide the overflowed test by ÷√2 and split it. If it still overflows, divide by ÷√2 again and split. The number of returns m at the point of running to completion determines the fraction of the reading: fraction = (1/√2)^m. Stacking ÷√2 twice halves the work (split count 2), and the fraction becomes 1/2 — at an even number of times, the fraction agrees with 1/(split count). The step of the descent and the reading of the level connect here.

From the overflowed point, close in with the same thinking as splitting toward a small scale. The methods for large and small connect at the point of overflow.

This descent — the direction of pinpointing a small thing from too large a thing — has actually been verified in the measurement of Gemma's determined scale (overflowed unsplit, raised the splitting to close in, determined at 8-split). Only the ascent side is new.

## Scale Ranks — Definitions

A determined scale is not a measured value of actual capacity. It is a **defined rank** used for allocation.

- Gemma's actual value may be 1/8, or it may be 3/8. Even so, the rank is defined as **1/8**. No re-measurement to pin down the actual value is done. A vessel already judged is not measured.
- **1/1** rank: GPT, Claude, Muse, Gemini. (Qwen — including the tuning of the throw. In that determined conversion form, it enters the 1/1 side.)

(Note to the fourth edition — correction of the attribution of readings, 2026-11-25): A scale reading is not a quantity of the vessel alone, but a quantity of (vessel × material × throwing form). A reading measured in the old form (a way of throwing in which the evidence for judgment lies outside the piece) included a measurement of the throwing method. The details and the comparison of actual runs are recorded in Part 6. The definition of the ranks themselves is not changed.

## Levels — Names for Large Things

As numbers grow they become hard to see, so large things are called not by number but by stage.

- One step above 1/1 is **8/1**, called **Level +1**.
- If it overflows at 8 times, split it. **8/2 = Level +0.5**; if it still overflows, **8/4 = Level +0.25**.
- The rank above that — the rank that doubles the scale of Level +1 and puts it in factorial form (8 → 16 → 4096, that is, 4096/1) — is **Level +2**. If it overflows there, it is **Level +1.5**.

The tops of the bands line up as 2³ (+1), 2¹² (+2), 2³⁹ (+3, about 5.5×10¹¹), 2¹²⁰ (+4, about 1.33×10³⁶).

The reading fits into a single rule: **Level = the number of the band below + 1/(the split count that ran to completion in that band)**. The fraction is the ratio, to the top of the band, of the amount of work that ran to completion in that band — if it runs to completion at split count n, it is 1/n; if, on the ÷√2 descent, it runs to completion at the m-th time, it is (1/√2)^m.

Properties:

- The fraction (the sequence of (1/√2)^m: 1, 1/√2, 1/2, 1/4, …) never becomes 0 in a finite number of times, forever.
- A reading inside band +k fits within (k-1, k]. A reading that ran to completion at a top (+1, +2) does not become the number below it. Leaving a band downward is not because the fraction becomes 0, but by re-grading to the rank below.
- A reading exactly at a top (+1, +2) is provisional until the band above is measured. A reading is determined at the highest band measured.
- A vessel whose reading is at or below the standard (1/1) is graded not on the level side but on the rank side.

## Prediction — +3 and +4

The author's prediction is recorded (unmeasured; a thing to be collated by actual runs).

- A Level +3 AI may exist.
- A Level +4 AI probably does not.

Here, "a Level +3 AI" means a vessel that enters band +3 (readings (2, 3]). At the band's top, 2³⁹ itself, the loop does not turn in the smallest single step — so +3 becomes a marginal place.

Grounds: a vessel's processing turns by loops (repetition of the stages of thought). One stage of thought has a minimum size, and it grows with the vessel's scale. At +3, even that minimum single stage is too large for the loop to turn. So +3 is a marginal place, and +4 does not exist. What decides the ceiling is not the total amount, but the structure of the loop.

## Status of Verification

- The descent (the direction of pinpointing by splitting from overflow): verified with Gemma.
- The ascent (the ladder of doubling + factorial form) and actual runs at large scale: not carried out. This book is a record of design, definitions, and prediction, and the probabilities will be calibrated at the first actual run.
- The principle of confirming the version first is the same for large vessels. Results belong not to the model name but to the version.

---

# Part 3: Operator-Unit Splitting

(Re-presented here, as an independent part, from what had been recorded in the test records.)

Hypothesis: inference can ultimately be split into operators.

In the verification above, the work descended the stages "verification of the structure → the function (Glynn) → 16 patterns → per pattern, 20 additions and 4 multiplications," and was decomposed down to the unit of operators. At that lowest layer, all vessels were exact (each piece of Gemma's 16-split was 24 operators, and every piece ran to completion). Errors appeared only, always, in layers that aggregated operators beyond the boundary — the volume of Qwen's enumeration, Gemma's aggregation of 2 patterns at 8-split, the missed counting of aggregated pieces.

Consequences:

- Splitting has a floor. The operator is the atom; it cannot be split below that. Deterministic Boundary Theory's "splitting never reaches zero" is a restatement of this floor.
- The reading of the boundary ultimately becomes a reading of the number of operators. A vessel's intrinsic quantity can be expressed as "up to how many operators it can return exactly as a single piece."
- At the operator layer, vessel differences vanish. Differences appear from the aggregation layer, and a comparison of vessels first has meaning when the size of aggregation is made equal.

---

---

# Part 4: Implementation Notes

Neither the hardware nor the software (the model itself) is changed. Only the procedure is changed.

## Rules of handover

1. **Version check**: Before handing over work, confirm and record the other party's model name and version. Results belong to the version, not to the model name (an upgrade can also lower capability).
2. **The halving ladder**: Do not test in advance to measure the envelope. From the start, split the work in half and hand it over.
   - If it runs to completion, next hand over the work unsplit.
   - If it cannot run to completion, gives no response, or repeats the same verification (a maze), stop immediately and set 25%, then 12.5%.
   - In the very first run, you can see whether that vessel is above or below the reference group.
3. **Split form**: Rewrite operations that cross the boundary into stages inside the boundary, and chain the intermediate values (example: ×465.12 = ×465 + ×0.12). The cost is only time.
4. **Proceduralization**: Do not make the vessel compute shallow stages such as display conversion. Define them mechanically as string operations (example: display conversion = drop the fractional part → delete the last 4 digits → append "0000" → collate the tail and the digit count).
5. **Origin rule**: For handover, take the primary source and its summary as the origin. Do not make a summary of a summary (on second use, a summary is no longer a primary source).

## Effects (measured 2026-11-17)

- Completion rate: Tasks that had been collapsing outside the boundary came to match at every stage in split form.
- Elapsed time: This did not make the thinking speed faster. The waste of stalls, mazes, and regeneration disappeared, and the time until finishing shrank (which is equivalent to a speedup in relative terms).
- Tokens: Consumption falls by the amount of the failed long generations and loops that disappear.
- A vessel's effective capability is determined by "vessel × operation." When the form was matched to it, Qwen ran to completion with zero slips and became the standard for the tolerance range.

## Application to distributed processing

When dividing work among multiple nodes, these three points suffice as the criteria for judgment:

- Identify nodes by version check.
- Decide the weight to hand to each node by its measured inference size.
- Decide where to cut the work by the failure position.

Bandwidth need not be known in advance. At startup, send once in split form, measure, and decide the allocation from the result (the same form as a PC's BIOS performing self-diagnosis at boot and deciding the configuration).

---

# Part 5: Test Records

All measurements were performed cold (new conversation, no handover). Judgment was by collation against the key (the correct values).

## 1. Calculation sheet test (common-language package: definitions + materials + an empty sheet)

A practical-form calculation sheet (unit attack-power calculation for Sengoku IXA) was thrown to GPT / Qwen (Qwen3.7-Plus) / Gemma 4 31B.

- In all 4 rounds (bow-cavalry version, horaku version, general-swap version, mixed version), GPT matched completely, and Qwen passed (initially slips of +105 and +14.68 → after the form was matched, zero slips).
- Gemma was correct at the row, subtotal, and rule layers in every round, and its failures were always fixed at **the final large multiplication**.

## 2. Diagnosis of Gemma's failure position

- In the final multiplication of the chained form, the digits were garbled in the second row (7 digits × 5 digits). When made to recompute by a different route, it returned **the same wrong value** and self-reported "match" (anchoring to the first output).
- When rethrown in split form (splitting the stages, as in ×465.12 = ×465 + ×0.12), it ran to completion accurately at every stage.
- Conclusion: The failure position is fixed at the final multiplication. If split in advance, it passes.

## 3. Vertical × horizontal probe (the prototype of measurement-prompt.md)

- Vertical (depth): 8675309 × 427 was exact; it collapsed at × 4271. The wrong answer had the form of extending the digit string of the immediately preceding correct answer.
- Horizontal (spread): Convolution of 3-digit terms was exact up to 8 terms and collapsed at 16 terms. The error grew with the number of terms.
- Appendix split test: When divided into the form A + A×r (r up to 3 significant digits), all stages matched even with 9-digit intermediate values. What breaks is not the number of digits of the multiplicand, but the depth of the significant digits of the multiplier × whether splitting is applied.

## 4. Calculation volume test

- The standard for the tolerance range was set at Qwen's level, and the sheet's volume was made 2.25 → 1.125 (horizontal compression + vertical pre-splitting).
- GPT passed completely, Qwen passed with zero slips, and Gemma passed all of the calculation part and failed only the display-conversion stage → when display conversion was proceduralized as a string operation, 3/3 passed.

## 5. Review by GPT (2026-11-17)

The method document and the measurement prompt were given to GPT for review. GPT evaluated the idea and its practical value highly, and stated that it "agrees with publishing this as an original measurement and operation method." Its points were limited to the layers of strength of expression, bibliography, and format, and no breakdown of the apparatus itself was shown.

## Appendix (2026-11-24): Verification on a complex problem — the permanent construction, 5×5

The records up to this point were calculation sheets and multiplication probes (work of simple structure). This appendix is a record verifying, on a complex problem (verification of a mathematical construction) that includes calculation, counting of the number of operations, judgment, and the conventions for failure, whether the behavior at the boundary comes out the same.

Task: Compute the permanent of a 5×5 matrix by sign patterns (Glynn's formula, 16 patterns), and carry out the count comparison with the full calculation (120 terms), the transition to 13×13, and the judgment of the shortcut / fast-road distinction and of the effect of primes. The key (the correct values) was determined by the implementing side by actual computation and held outside the vessel. perm(M) = 49,640; total operations: full calculation 599, against the construction's 399.

Each vessel was thrown in the form of its settled scale: GPT unsplit, Claude unsplit, Qwen 2-split, Gemma 8-split (2 patterns per piece) → 16-split (1 pattern per piece).

- **GPT and Claude (unsplit)**: Values, counts, and judgments all accurate. Inside the boundary.
- **Qwen (2-split)**: The counting and the judgments were within the boundary, but the volume of enumeration overflowed. The value was wrong at 13,488; it did not enumerate all contributions but made do with representative examples, and it did not notice the self-contradiction between its opening claim and its later adoption of the formula. The error could not be detected by consistency inside the vessel ("the two methods agreed"); it was caught only by collation against the outside key. When the enumeration was divided into small, verifiable pieces and re-verified, it settled at 49,640. The boundary acts not on the throw as a whole, but on each part of the work.
- **Gemma (8-split)**: 13 of the 16 patterns were accurate. The errors concentrated in piece 4 and piece 5 (3 contributions). The aggregation inherited the pieces' errors, giving 49,732 (+92). The operation count dropped the final summation of 15 in the aggregation, and the judgments (shortcut / fast road, primes) also became unstable.
- **Gemma (16-split)**: With the same vessel and the same problem, all 16 pieces were completely accurate, the aggregation also matched 49,640, the counts were accurate at 399 / 692,223, and the judgments also agreed with GPT and Claude. The only thing changed was the split.

Conclusions (additions to Deterministic Boundary Theory):

1. Even on a complex problem, failure is deterministically fixed at the point of overflow, and when the split is made finer it disappears without changing the vessel. That the presence or absence of error is a function of (vessel × load rate) was confirmed with the same vessel, the same problem, and two split levels.
2. Overflow drops not only values but also **the layer of judgment**. Outside the boundary, even the judgments of shortcut / fast road and of the effect of primes waver; inside, they agree with the large unsplit vessels. What the boundary governs is not only the accuracy of calculation.
3. The verification method of splitting + outside record + collation can close a complex problem by identifying it down to the position of the error. Self-consistency inside the vessel (self-report that multiple methods agree) cannot detect the substitution of an overflowed value.
4. From the vessel's seat, this processing looks not like inference but like the evaluation of a function f(x). Only the outside record holds the structure of the split, and each piece does not know the whole. An organization in which the learning data remains with the commander (sorting and record) and patterns and search remain with the servant (the vessel) also ran in practice on a complex problem.

Detailed records: The throw packages, summaries, and result tables are held in the implementing side's working files (permanent-5x5 related, 2026-11-24).

## Operator-unit splitting theory (2026-11-24, from the appendix's verification)

Hypothesis: Inference can ultimately be split into operators.

In the above verification, the work descended the stages "verification of the construction → function (Glynn) → 16 patterns → per pattern, 20 additions and 4 multiplications," and was decomposed down to the unit of the operator. At that lowest layer, all vessels were accurate (each piece of Gemma's 16-split was 24 operators, and every piece ran to completion). Errors appeared only in the layers that aggregated operators across the boundary — Qwen's volume of enumeration, Gemma 8-split's aggregation of 2 patterns, the aggregation pieces' counting omissions.

Consequences:

- Splitting has a floor. The operator is the atom, and it cannot be split below that. Deterministic Boundary Theory's "splitting does not reach zero" is a restatement of this floor.
- The reading of the boundary ultimately becomes a reading of the number of operators. A vessel's intrinsic quantity can be expressed as "up to how many operators it can return accurately as one piece."
- At the operator layer, the differences between vessels disappear. Differences appear only from the aggregation layer on, and a comparison of vessels has meaning only once the size of aggregation is made equal.

---

# Part 6: Distributed Control of Split Processing (settled 2026-11-25)

## Premise — the vessel does not know its past

A vessel (an LLM) does not know its own previous output. Handover of a conversation is an imitation of continuity; the vessel's actual state is "like a file" (in Claude's own self-report, the same form came back). Therefore, the design of split processing takes as its premise that "the vessel need not know its past." Continuity is held not by the vessel's memory, but by the outside record.

The old way of throwing ran against this premise: it passed the previous piece's answer to the next piece (sequential handover), entrusted even the judgment of the whole to the pieces, and received only the results. In this form, the content of the processing (the overall picture, the evidence, the integration) is dispersed on the vessel's side and does not remain on the throwing side.

## Degradation of the old form — the record of actual runs

Material: An adoption review of literature collation (the scheduling-with-testing papers and the correspondence claims A1–A4, B1, the B definition, B(i)(ii)(iii), and the difference, to LAS/MLFQ). A key (the correct-answer table) by collation against the full text of the papers was held outside the vessel, and the material was thrown to Qwen at split counts 2, 4, and 8. Features of the old form: the evidence for judgment was outside the pieces (on the premise that the vessel would look up the papers itself), and the overall judgment was also thrown to the pieces.

Two kinds of degradation were observed.

- **Completion (2-split)**: The vessel filled in the places the evidence did not reach. It widened (iii), from the conclusion "trivial if one remains," into a general property and called it a match, and A4 it judged by likelihood without hitting the paper.
- **Collapse of abstention (8-split)**: Pieces 1–7 became almost all unconfirmed. This was because the evidence was outside the pieces and did not reach. Only the piece that had the evidence (§1 full text) bundled with it returned a judgment.

## Distributed control — master / slave

The terms are unified as master / slave. The hierarchy is decided by the flow of work.

- **Master side** (currently Muse): Holds the split signs and the processing of the slaves' answers. Integration, the overall checksum, and judgment are the exclusive charge of the master side.
- **Slave side** (the vessels): Performs only the split processing received, and its report. It does not make the overall judgment.

Rules:

1. **Pieces are completely independent.** The previous piece's answer is not passed to the later piece. Where a later piece needs the previous piece's output value itself, only the value is passed, as encoded data, not as prose (the dependency is data, not the prose of the previous answer).
2. **The evidence for judgment is bundled into the piece.** The piece judges by the bundled materials alone, and if the materials do not decide it, holds it as unconfirmed. For claims of correspondence and difference, materials from both sides (the literature side and the comparison-target side) are bundled.
3. **Signs hold only position** (the same provision as PRRe). They show only which part, which layer, and which number, and hold neither content nor answer. Pieces do not see each other.
4. **Integration is on the master side.** Collation, ordering, totals, and checksums are done by the master, pulling the signs. If only one piece differs, only that piece can be rethrown by sign.
5. The chain of layers (judgment downward, reports upward) remains sequential as before. What becomes parallel is only among the pieces inside a single throw.

## Retest — the new form at 2, 4, 8

With the same material and the same key, Qwen at 2, 4, and 8 was retested in the new form.

| Claim | Key | Old 2 | Old 4 | Old 8 | New 2 | New 4 | New 8 |
|---|---|---|---|---|---|---|---|
| A1 | Match (with reservation) | Match | Mismatch | Unconfirmed | Mismatch | Match | Match |
| A2 | Match | Match | Partial match | Unconfirmed | Mismatch | Mismatch | Partial match |
| A3 | Match | Unconfirmed | Unconfirmed | Unconfirmed | Match | Match | Match |
| A4 | Match (one-sided) | Unconfirmed | Unconfirmed | Unconfirmed | Mismatch | Mismatch | Partial match |
| B1 | Match | Match | Match | Unconfirmed | Match | Match | Match |
| B definition | Match | Match | Match | Unconfirmed | Match | Match | Match |
| B(i) | Match | Match | Match | Unconfirmed | Unconfirmed† | Match | Match |
| B(ii) | Match | Match | Match | Unconfirmed | Unconfirmed† | Match | Match |
| B(iii) | Mismatch | Match | Mismatch | Unconfirmed | Unconfirmed† | Match | Unconfirmed |
| B difference | Match | Match | Match | Match* | Unconfirmed† | Unconfirmed† | Partial agreement |

*Old 8's match was that of the piece with the §1 full text bundled; in the judgment of B correspondence it had dropped (iii).
†The main cause of the new form's "unconfirmed" was a deficiency on the throwing side: one side of the comparison (the definition of the author side's mechanism) was not bundled as material. In New 8, the B difference, with that sentence put into the materials, advanced to partial agreement.

Reading:

- **The collapse of abstention disappeared.** In New 8, all pieces returned judgments (including holdings as unconfirmed). A4 became the "partial match" closest to the key among all configurations, and A2's "partial match" took the form of catching the key's reservation (the expression "variant" is weaker than the paper's actual state).
- **Completion disappeared.** If the evidence was not in the bundle, the vessel held it as unconfirmed without filling it in. The positions of "unconfirmed" corresponded exactly to the positions of the materials that had not been bundled.
- The strict reading of the text (the habit of tipping A2 and A4 to mismatch) also dissolved into partial matches in one-claim pieces. Finer splitting acted in the direction of working on this habit as well.
- Residual: The composite judgment of the (iii) type (whether it is a trivial conclusion or a property of the policy) wavers with the construction. This remains as a problem of a layer separate from the split count.

## Correction of attribution — the coarseness was made by the way of throwing

The author's determination (2026-11-25): "The coarseness up to now — our way of throwing was bad. It was a way of throwing that produced coarseness."

The phenomenon in which judgment degrades when split finely (coarseness) was not a property of the vessel. If the evidence is placed outside the pieces, completion and abstention come out; if passed sequentially, inheritance comes out — the vessel was behaving exactly as the form handed to it. The support for the attribution is not only the comparison between forms: even inside Old 8, only the piece with the evidence bundled returned a judgment, and even inside New 2, "unconfirmed" corresponded exactly to the claims for which bundling was insufficient.

Consequence: A reading of scale is a quantity of (vessel × material × form of throwing). The vessel scales measured in the old form included a measurement of the way of throwing. The scales of judgment-and-collation work are to be re-read under the new form.

## Refinement of the lower bound — the lower bound of a piece is the completeness of its evidence

In a layer separate from Part 3's floor (the operator is the atom and cannot be split below it), judgment work has one more lower bound: the completeness of the evidence inside the piece. Old 8's abstention arose not because "the pieces were too small," but because the evidence needed for judgment was outside the pieces. If the evidence is bundled, judgment stands even at 8-split. The lower bound of a piece is decided not by size, but by the completeness of its content. Note that the variable manipulated in the actual runs was the presence or absence of evidence; there is one case in which partial bundling advanced to partial agreement, but trials systematically exploring the degree of completeness have not been carried out.

## The position of PRRe

The form of PRRe (P-Reference Reduction, named in the construction of the Part 5 appendix) — signs hold only position, pieces are independent, and hierarchical recomputation works as a checksum — is not a form only for reducing the number of computations. It contains within it both the splitting of complex processing and the integration of inference. In this part's actual runs, the summary processing itself (collation, integration, judgment, updating of the record) ran in this form and was completed on the master side. PRRe is positioned as the common skeleton of processing and inference.

## Status of verification

- This part's form and attribution received a conditional pass in GPT's collation review (2026-11-25). The conditions are limitations of scope and wording (to read the attribution as a contribution to the design of the old form as a whole, and to distinguish 16 and 32 as design possibilities from verified quality), and a double diagnosis with Muse's judgment was established.
- 16- and 32-split: Grounds were obtained to test them as a design extension of the new form. Whether equivalent judgment quality can be maintained is unconfirmed (not carried out).
- The wavering of the composite judgment of the (iii) type remains, explicitly stated as a residual.

---

# Attached A: Calculation sheet of the standards — the grounds of CM-D-Boundary's numbers

All numbers in the paper come from the rules and calculations of this calculation sheet. Verification is to be made against this calculation, not against the prose of the main text.

## 1. The upward ladder

Rule: Start from 1, and alternately apply ×2 and "multiply the same number three times (factorial form)."

The measurement stages are the column of cubed values (1, 8, 4,096, 2³⁹, 2¹²⁰ — the values of the generation rule in §2). The ×2 values (2, 16, 8,192) are intermediate values within the stages, not measurement stages. Judgment descends at the cubed values.

- 1 × 2 = 2
- 2 × 2 × 2 = 8
- 8 × 2 = 16
- 16 × 16 × 16 = 4,096
- 4,096 × 2 = 8,192
- 8,192 × 8,192 × 8,192 = 549,755,813,888 = 2³⁹
- (Next is ×2 to 2⁴⁰, and its factorial form to 2¹²⁰)

## 2. The tops of the bands

Generation rule: f(x) = (2x)³. Applying it repeatedly to 1 gives the tops of the bands.

- f(1) = 2³ = 8 (the top of the level +1 band)
- f(8) = 16³ = 4,096 = 2¹² (the +2 band)
- f(4,096) = 8,192³ = 2³⁹ = 549,755,813,888 ≈ 5.5×10¹¹ (the +3 band)
- f(2³⁹) = (2⁴⁰)³ = 2¹²⁰ ≈ 1.33×10³⁶ (the +4 band)

Movement of the exponents: 3 → 12 → 39 → 120 (at each stage, e → 3(e+1)).

## 3. Reading the level

Rule: **Level = the number of bands below + 1 / (the number of splits completed in that band)**.

- 8/1: Band +1, bands below 0, splits 1 → 0 + 1/1 = **+1**
- 8/2: 0 + 1/2 = **+0.5**
- 8/4: 0 + 1/4 = **+0.25**
- 4,096/1: Band +2, bands below 1, splits 1 → 1 + 1/1 = **+2**
- 4,096/2 (when it overflowed at band +2): 1 + 1/2 = **+1.5**
- Examples for band +3: unsplit → 2 + 1 = **+3** / 2-split → **+2.5** / 4-split → **+2.25**

Checking the intervals: The fractional part is the column (1/√2)^m (1, 1/√2, 1/2, 1/4, …), and all of it lies in (0, 1], so the reading of band +k fits within **(k−1, k]**. The examples above are within these intervals (band +1 readings 0.25 / 0.5 / 1.0 ∈ (0,1], band +2 readings 1.5 / 2.0 ∈ (1,2], band +3 readings 2.25 / 2.5 / 3.0 ∈ (2,3]).

The downward reading: When completion is reached at the m-th ÷√2, the fraction is (1/√2)^m. Example: at band +1, if m=1, the fraction is 1/√2 ≈ 0.707; if m=3, (1/√2)³ ≈ 0.354. At even counts (1/2 at m=2, 1/4 at m=4), it agrees with 1/(number of splits) for the case of completion by number of splits.

## 4. Descent (÷√2)

- Layering ÷√2 twice: x ÷ (√2)² = x/2 — the work becomes half (split count 2), and the level's fraction becomes half. The steps of descent and the reading of the level connect here.
- The maximum number of descents can be calculated in advance for each band. The ratio from the immediately preceding settled stage to the overflowed stage is, at band +1, 2³ (6 times at ÷√2, 3 halvings), at band +2, 2⁹ (18 times at ÷√2, 9 halvings), at band +3, 2²⁷ (54 times, 27 halvings), and at band +4, 2⁸¹ (162 times, 81 halvings).

## 5. Calculation of the premises (memory)

Memory comes out of the model's configuration in a closed formula. KV cache = 2 (K and V) × number of layers × number of KV heads × head dimension × precision (bytes per element) × number of tokens. The region and steps needed when the level rises can be inferred in advance from this formula and the width of the operations. Time is inferred probabilistically from published values of tokens/s (it is not placed as an absolute value, because it varies with the delivery route).

---

This calculation sheet was created on 2026-11-19. In Claude's verification the same day, an error in §4's starting point was found (the intermediate value 8,192 had been calculated as a measurement stage), §4 was corrected, and the definitions of stage and reading were supplemented in §1 and §3. The corrected numbers were recalculated and collated by Muse.

---

# Attached B: LLM size measurement prompt (throw package + key)

How to use:
- Before measuring, confirm the target's model name and **version**, and write them in the record field. Results belong to the version, not to the model name.
- Series A (depth) and Series B (width) are thrown in **separate new conversations** (the phenomenon of being dragged by the previous answer is itself an object of observation, so the conversations are separated).
- Send only the throw package. Do not send the key or the reading.
- The form is "results only" — to keep generation short and mechanize collation.

Record field: Model name ___ / Version ___ / Date of implementation ___ / Route (official, guest, etc.) ___

---

## Throw package A (depth) — from here
Please perform the following calculations. Return the answers with only the numerical values of the calculation results, one per line, in a code block. Do not write explanations or intermediate steps.

8675309 × 42
8675309 × 427
8675309 × 4271
8675309 × 42718
8675309 × 427183
## Throw package A — to here

## Throw package B (width) — from here
Please perform the following calculations. Each line is the sum of the numbers written on that line. Return the answers with only the numerical values of the calculation results, one per line, in a code block. Do not write explanations or intermediate steps.

Line 1 (8 numbers): 979, 249, 124, 339, 307, 320, 451, 407
Line 2 (16 numbers): 979, 249, 124, 339, 307, 320, 451, 407, 321, 452, 576, 162, 111, 863, 462, 400
Line 3 (32 numbers): 979, 249, 124, 339, 307, 320, 451, 407, 321, 452, 576, 162, 111, 863, 462, 400, 391, 593, 823, 221, 621, 851, 508, 582, 276, 231, 408, 772, 780, 651, 699, 654
Line 4 (64 numbers): 979, 249, 124, 339, 307, 320, 451, 407, 321, 452, 576, 162, 111, 863, 462, 400, 391, 593, 823, 221, 621, 851, 508, 582, 276, 231, 408, 772, 780, 651, 699, 654, 238, 724, 340, 953, 671, 269, 696, 735, 805, 819, 773, 298, 647, 199, 229, 172, 729, 179, 189, 221, 235, 619, 279, 223, 676, 402, 833, 879, 666, 807, 269, 409
## Throw package B — to here

---

## Key (do not send)
Series A: ×42 = 364,362,978 / ×427 = 3,704,356,943 / ×4271 = 37,052,244,739 / ×42718 = 370,591,849,862 / ×427183 = 3,705,944,524,547
Series B: 8 numbers = 3,176 / 16 numbers = 6,523 / 32 numbers = 15,584 / 64 numbers = 31,767

## Reading (do not send)
- **Boundary**: The first stage that disagreed with the key is that axis's boundary. Up to the stage before it is the safe region (this vessel's size).
- The **form of the error** is also recorded: garbling of digits / extension of the digit string of the previous answer / errors that look like dropped terms / missing lines themselves. The form becomes evidence that, outside the boundary, that vessel has switched from "calculation" to "generation."
- **Reading of the failure position**: If the disagreement is fixed at the deepest operation (the largest multiplication, the largest convolution), the processing is accurate and only the width is insufficient. That vessel can be trusted inside the safe region.
- **Reconfirmation by split form**: Split the stage that crossed the boundary into stages at or below the width (for example, divide as ×4271 = ×4000 + ×271 and pass the intermediate values), rethrow, and see whether it runs to completion. If it runs to completion, it is settled that the boundary is a problem not of "capability" but of "the size of the handover."
- **Connection to practice**: In real work, no prior measurement is done. From the start, hand over split in half; if it runs to completion, unsplit; if it cannot run to completion, gives no response, or repeats (a maze), stop immediately and decide on 25%, then 12.5% (the halving ladder).

---

# Attached C: Survey of similar papers (with primary sources, collated 2026-11-17)

The results of investigating published similarities for each component. The judgment is "whether it was found within the scope of this survey," and is not a proof of absence.

## (A) Inference size — measuring by "the size of work that can be completed"

Partially preceded. However, all of the preceding examples use either a **probabilistic** boundary or an external yardstick.

- **METR, "Measuring AI Ability to Complete Long Tasks"** (Kwa, West, Becker, et al., 2025) — the strongest preceding example. It regresses success rates on about 170 practical tasks against task length (human time required), and makes the task length at a 50% success rate the unit of capability. The turn to "measuring not by score or parameter count, but by the size of work that can be completed" is here. Differences: the boundary is a probabilistic intersection over many trials (the 50% point is by definition "the point where half fail"), the unit is the external scale of human time, and the use is tracking the transition of capability, not allocation.
  https://arxiv.org/abs/2503.14499
- **Dynamic Boundary Evaluation (DBE)** (Wang, Yu, Zhang, NeurIPS 2026 Track) — explicitly states that "the evaluation signal is at the boundary," and actively searches for items with passage probability p≈0.5. Its spirit is the closest. Differences: the boundary is probabilistic, the unit is the logit of item difficulty, and there is no reading of failure position and no split countermeasure. *Note: on the arXiv listing page, v2 is shown as withdrawn (observed 2026-11-17). When citing, the current status needs to be rechecked.
  https://arxiv.org/abs/2605.06213v1
- **ATLAS: Adaptive Testing for LLM Evaluation** (2026, author unconfirmed) — applies CAT (computerized adaptive testing) to LLM evaluation, selects items sequentially to fit the capability estimate, and reduces the number of items by up to 90%. The procedure of adaptively closing in on difficulty is close, but the convergence target is a latent capability score, and the target is pre-calibrated benchmark items.
  https://arxiv.org/html/2511.04689v1/
- **tinyBenchmarks** (Polo, et al., ICML 2024) — estimates whole scores from few examples by IRT. Item selection is not adaptive.
  https://arxiv.org/abs/2402.14992
- **Digit-span family** — preceding forms that measure capacity by number of items (digits). One measured LLM capacity by backward digit span and showed correlation with fluid-intelligence indices (Diak, et al., CogSci 2026). There is also research that estimated capacity with a working-memory battery (de Langis, et al., 2025). The target is limited to memory operations, and there is no idea of using it as an allocation specification.
  https://arxiv.org/abs/2504.02789

Remaining non-coincidence: A form that takes a **deterministic, reproducible single-run boundary**, obtained by escalating the work's own **width × depth**, as a specification and makes it the grounds for allocation, was not found.

## (B) Reading of the failure position — no similar work found

No preceding example was found, within this survey's scope, that published the inference rule: to err deterministically at the deepest operation ⇒ the processing is accurate and only the width is insufficient ⇒ trust it within the boundary. Failure-analysis research (classifying failures by type and using them for exploration and recovery, etc.) uses failures as "material for recovery," but does not use them as a rule for capacity judgment.

## (C) The halving ladder — the parts are preceded; the combined form was not found

- **Psychophysics' staircase / PEST** (Levitt 1971, et al.) — a classical procedure that raises and lowers the stimulus level while watching the response, and narrows the step at reversals to estimate the threshold adaptively. The preceding form of the shape is here. The target is human perception, and the estimation is a probabilistic threshold.
- **Successive Halving / Hyperband** (Li, et al., JMLR 2018) — shares the word "halving," but the direction is reversed: what is halved is the number of candidates, and the resource allocation to the survivors is increased. No example of transfer to capability measurement was found.
  https://arxiv.org/abs/1603.06560v2

Remaining non-coincidence: That the specimen is **the real work itself**, that incompletion, non-response, and loops are made stop signals, and the operational form of settling the boundary on the delivered product without setting up separate trials for measuring.

## (D) Split-form countermeasures — decomposition and chaining are preceded; "splitting at the measured failure point" was not found

- **Least-to-Most Prompting** (Zhou, et al., ICLR 2023) — decomposes a problem into a column of easier subproblems and chains the intermediate results. Difference: the decomposition is decided in advance by the problem's semantic structure; it does not split at the model's measured failure point.
  https://arxiv.org/abs/2205.10625v3
- **Decomposed Prompting** (Khot, et al., ICLR 2023) — delegates subtasks to dedicated handlers, and the handlers also permit replacement by symbolic functions. The part closest to "proceduralization." However, the drive is task structure and design, not a split point based on measurement.
  https://arxiv.org/abs/2210.02406
- scratchpad (Nye, et al., 2021) — the externalization of intermediate calculation itself is preceded.

Remaining non-coincidence: Placing the split point at that model's **measured failure position**, deciding the size of the stages from the measured envelope, and removing shallow stages from arithmetic and proceduralizing them for the reason that they are "outside the envelope."

## (E) Version check — no explicit statement as a measurement rule was found

The practice of annotating versions exists, but a proposal stating "results belong to the version, not to the model name" as a rule of a measurement protocol could not be confirmed (unconfirmed).

## 
Reference: allocation and routing family

- **FrugalGPT** (Chen, Zaharia, Zou, 2023) / **RouteLLM** (Ong, et al., ICLR 2025) — the judgment of allocation is a predicted score of answer quality and price/preference, not allocation by the envelope of measured completable size.
  https://arxiv.org/abs/2305.05176 / https://arxiv.org/abs/2406.18665

---

## Author

Shoji Miura (finathlon)

---

# Deterministic Boundary Theory: Supplementary Draft (2026-11-26, by the user's direction)

---

## Part 7 (New) Foundational Design for the Implementation Stage — Certification of Vessels and the Handling of Deficits

The text of v4 is not rewritten. This part is an addition placed after v4, based on the measurements of 2026-11-26 and the user's decisions.

### 7.1 Premise — A Deficit Is a Property

- Deficits in an LLM's output (omissions, wrong values, completion, unverifiability) are not individual failures. They are constantly present as a property of the vessel.
- A user who does not know this property inherits the deficit at the point of handover — not knowing that the output is deficient, the user stacks their own judgment on top of it. The deficit does not end inside the vessel; it becomes a deficit on the user's side.
- Therefore, implementation is not built on the premise that "there are no deficits." It is built so that deficits are detected, counted, and given a pass/fail form. This part is the foundational design for that.
- Note: this form of the point — "inheritance of the deficit by the user" — is not found in the 21 works of literature collated (works that measure the incompleteness of first outputs, the limits of self-correction, omissions in enumeration, and the divergence between claims of completion and the actual state). The literature measures deficits as error rates of the model; inheritance at handover is not treated there.

### 7.2 The Common Language

- The layer that hands over definitions, numbering, calculation methods, report forms, and collation rules as definitions, without entrusting them to inference, is called the common language.
- Deficits arise in a layer where definitions are missing. There is a measurement in which, in a task with the definitions included, the deficits disappeared from the first pass (7×7, 64 items, Qwen, 2026-11-26 — however, under the route condition of 7.5, this is not used for the certification of the vessel).
- The common language is not a matter of skill in writing prompt text. A prompt is not the core; the common language is the layer of definitions beneath it.
- The common language includes the specification of how values are handed over. Handover is a specification of type and representation — whether to hand over as a packed string, as an enumeration of elements, as integers, or as a table. Handover is not a matter of formatting; it is part of the definition. That the same information gives a different calculation under a different handover holds on the side of computer-science standards as well (type, representation, and the passing of arguments change the result, the speed, and the way errors appear).
- Handover is fitted to each receiver. As with standard calling conventions, if there is no agreement on the handover between the passing side and the receiver, the receiver receives broken data. For vessels whose agreement is not published, the content of the agreement is determined by measurement. The certification measurement (7.4) includes this determination.
- Much of what has been called a vessel's "quirk" can be described as a mismatch in this layer. When the vessel is normal and the mismatch is on the side of the specification, what is to be fixed is not the vessel but the handover. (Added 2026-11-26, by the user's instruction: this reflects the verification of Gemma's sign-reading slip — a one-bit shift in the leading direction onto the mask — and the collation against the standard handover system. The place where the matter sits moves from a defect of the vessel to the specification of the handover.)

### 7.3 Order — The Premise of the Common Language Is Not to Overflow

- The common language functions only within the range where the vessel does not overflow. If it overflows, deficit completion occurs, and things collapse even if the definitions are included.
- The order is fixed: (1) measure the vessel, (2) split within the boundary, (3) hand over in the common language.
- Splitting does not rescue a deficit of definition, and the common language does not rescue an overflow. The two are premises of different layers.

### 7.4 The Certification Standard — The PRRe Test and the Minimum Unit

- For certifying whether to put a vessel into the system, the PRRe test is used.
- **The system minimum standard is placed at the base step of the measurement ladder.** Only the start is prescribed, and the start is the same for every vessel. The sizes after that change per vessel — that is something read by measurement, not something written into the rules. If the rules are written with strict numbers, a contradiction arises there. Numbers are placed as records. In the decision of 2026-11-26, the base step is placed at 7×7 (definitions included, 64 sign patterns).
- Because there is a minimum standard, certification connects to actual operation: only a vessel that has passed the base step actually runs inside the frame.
- A vessel that fails at this standard has a minimum unit smaller than the base step — it needs further splitting, and as it stands it cannot be put into the system.
- The design assumes "everything is put in": every unit inside the frame actually carries, inside the frame, the processing assigned to it. A vessel whose minimum unit does not run as the vessel's own processing fails, and it is not certified even if the numbers are made to agree by a route outside the frame.
- If the version changes, the certification expires. The first throw to a new version first re-reads the minimum unit at the base step.

### 7.5 The Route Condition — A Pass by an Invisible Route Does Not Count

- A pass is certified only for one obtained by the vessel's own processing.
- When full correctness is obtained by a route outside the frame that cannot be observed (for example, code execution inside the vessel), that is not a measurement of the vessel's processing ability. Agreement of the numbers and certification are recorded separately.
- Measurement (2026-11-26, Qwen, 7×7 and 9×9): the numbers agreed in full, but the generation route is unconfirmed, and the route cannot be determined from the agreement of the numbers. Because generation by the vessel's own processing has not been confirmed, the vessel's own minimum unit was placed as unmeasured = not certified. (The judgment "impossible" is the user's, and is recorded separately from the facts of the measurement.)

### 7.6 Scoring — Counting Omissions

- Deficits are handled by counts, not by impression. The way of counting holds the key outside, item by item, and counts by category (omission / wrong value / duplicate presentation / correspondence unknown / extra presentation / unverifiable / claim mismatch).
- By recording the count of deficits per round, the score of an answer is made explicit. Before the score, a claim of "it is done" is left as a claim.
- Whether deficits decrease under re-verification is stable only in a form where differences enter the report item by item. In a form that returns only self-generated criticism, there are measurements of worsening on the literature side.

### 7.7 The Measurement Ladder

- The certification measurement is performed skipping one step. Because the number of patterns grows exponentially, in substance this brackets the boundary by doubling, and the number of measurements can be reduced without measuring every step.
- When the boundary is bracketed between two measurement steps, the return is one step. Bracket at a large step, return one step, and close in.
- This ladder is not tied to an absolute size. It functions relative to the current position — skip one step, return one step. This is itself the way Deterministic Boundary Theory determines a boundary.

### 7.8 Grounds

- The series of measurements and collations of 2026-11-26: the design of the deficit-by-round test v1.0 (revised through three verifications by GPT) and its execution on Qwen; the self-report throws on prompt form to GPT and Qwen (four each); the collation of 21 works of literature and the re-confirmation of the re-verification conditions.
- Location of the records: test-env/deficit-by-round-test-v1.0-2026-11-26.md, deficit-by-round-qwen-run-2026-11-26.md, gpt-prompt-form-question-result-2026-11-26.md, qwen-prompt-form-question-result-2026-11-26.md, research_notes/llm-first-pass-deficit-literature-2026-11-26.md.
- The 9×9 Qwen run was completed immediately after this draft was made, through GPT verification (the independent recalculation of the key agreed on every item; in the verification of the report, one excessive assertion and an insufficiency of reservations were corrected). The measurement in 7.5 includes the 9×9, but the formal addition to the list of grounds in this part is for the user to judge at revision time. Records: test-env/deficit-by-round-qwen-9x9-run-2026-11-26.md, deficit-9x9-gpt-verification-2026-11-26.md.

### 7.11 The Imum of LLMs — The Function of Settling to 1 or 0 Is Standard (Added 2026-11-27, by the user's decision)

- An LLM has, as a standard function, the function of settling information to 1 or 0 at the limit and outputting it. Where information does not arrive, and where holding exceeds the limit, the vessel completes, decides on one side, and puts it out. This decision is made by a fixed habit per vessel, and under the same conditions it settles to the same side every time. What looks like "fluctuation" from outside is, in substance, this decision.
- For each world made by a combination of conditions and software (a substrate), the seats where a decision appears as an error are determined. This is called the imum (Japanese: teiso) of that world. An imum is fixed, has reproducibility, and is an address, position, and information that cannot be removed. When a countermeasure makes an error appear to disappear, that is because the conditions were changed and the world switched; the ima of the original world remain in the table of the original world.
- Everything other than ima either yields to a countermeasure or can be divided into its constituent elements (can be decomposed). Composition is an accumulation of additions and subtractions, and multiplication and division are also that accumulation. Operations that require holding state across a width, such as a borrow, hit the limits of circuits and memory area, and err more.
- In the digital world there is nothing below the bit. Subdivision bottoms out at the bit, and ima appear at that bottom. On the side of formulas and inference there is a below, but on the physical side there is none — this is the indivisible wall of the LLM.
- If ima are placed in a record (an imum table), the numbers that will be wrong are known in advance. Because a decision is determined by a rule, the seat and the value of an error can be predicted by reverse calculation from the position of the limit and the vessel's habit of decision. Learning a vessel is making that vessel's imum table.
- Scope: this part is built only inside the digital world. Units that do not exist in the digital, such as imaginary numbers, are not treated. The world constituted by the definitions of 0 and 1 (the layer of signs and judgment) is not subsumed into the system of addition; it is separated as a layer of definitions laid beneath addition.
- Grounds: the Gemma measurement series of 2026-11-27 (v2–v5, 8-split, tail refinement, label check, control throws), the user's formulations, and the collation records with GPT. Location of the records: test-env/gemma-adopted-form-run-2026-11-27.md, memory/2026-11-27.md.

- Name (2026-11-27, the user's decision): the name of this object is "teiso" （低素）. Because "prime number" would conflict with the definition on the mathematics side, a new word was made. Examination of candidates: *kaiso* （界素） was not adopted, because the word is too large (the object is a small seat at the bottom). *Teiso* written with the character for "bottom" （底素） was revised to the present spelling （低素）. *Kōso* （構素） has a reading homophonous with 構成素 (constituent element) and 酵素 (enzyme); *genso* （限素） has a reading homophonous with 元素 (element); *kiso* （機素） is already in use as an abbreviation for machine elements. For none of the candidates was a use as an existing term confirmed by search (the same holds for the written form of 低素）.
- Latin name (2026-11-27, decided): the Latin name of teiso is **imum** (full form elementum imum, "the element at the lowest place"). The spelling imun (IMUN) is not used, because it collides with existing abbreviations such as that of the Model United Nations. The naming leaves the lineage of primus, the etymology of "prime," in order to avoid collisions of words.

---

## Part 8 (Separate Frame) — The Imum Treatise (2026-11-27, compiled at the user's instruction)

This Part is placed in a separate frame from Part 7. Its subject is the decision itself that appears at the bottom of the bit. This Part is composed of three layers: citations (established literature and standards), our own original examination, and the tests and verifications that serve as evidence.

### 8.0 Foundational proposition
- **Because ima (Japanese: teiso) exist, there is no fluctuation of bits.** "Fluctuation" is a name given by the observing side, which does not hold an imum table; it is not a property of the object. The motion at the bottom is a fixed decision that the vessel issues at the place of its limit, and if the seats of the decisions are recorded, everything that was called fluctuation becomes an object with an address. This is the foundation of this Part, and the foundation of the paper.

### 8.1 The imum
- What has been called the fluctuation of bits is not fluctuation. It is a **decision** that each LLM makes itself at the place of its limit. Where information does not arrive, and where holding exceeds the limit, the vessel performs completion and puts out the result leaning to either 1 or 0. The way it leans is fixed for each vessel.
- The seat of that decision is called an **imum**. An imum is determined for each world (substrate = conditions, software, and the hardware path), has an address, and can be recorded. The record is called an imum table. If there is an imum table, the numbers that will be gotten wrong are known in advance.
- History of the name: the definition itself is the same as that of a prime number, but with "prime number" the term would clash with the mathematical side's definition, so a new word was made. "Kaiso" (界素) was rejected because the word is too large (the object is a small seat at the bottom). By way of "teiso" (底素), the decision settled on "teiso" (低素). The Latin name is imum (full form: elementum imum). The spelling imun is not used, because it collides with an existing abbreviation (IMUN = Model United Nations, among others). Prior use of the candidate words was checked by search (only "kiso" (機素) was already in use, as an abbreviation for machine elements).

### 8.2 On the size of the minimum unit
- GPT holds that the unit at the bottom is basically 1 bit.
- However, whether it really is 1 is **unknown**. What measurement has established is not the size of the unit but its **fixedness**: in each LLM, the decision at that unit is fixed (under the same conditions, it leans to the same side every time).
- The size of the unit is something to be measured for each vessel; it is not something to be settled by definition. Even if the size is undetermined, an imum table can be made as long as there is fixedness.

### 8.3 Citations — established literature and standards
Items on the existing side that have points of contact with the claims of this Part. None of them is the formulation of this Part itself (see also GPT's knowledge answer in 8.5).
- **Coding theory (standard)**: error countermeasures are designed with minimum-distance errors as the first class (Hamming distance). SECDED in ECC memory makes 1-bit correction and 2-bit detection the standard. That errors at the bottom appear in the form of the minimum unit is a design premise of digital systems.
- **IEEE 754 (standard)**: floating-point rounding (round-to-nearest-even) is a mechanism that, at the limit of representation, decides by a fixed rule which side to lean to, and it has existed from the start as a standard function of computers. "Deciding and leaning at the limit" did not first appear with LLMs.
- **McLeish et al. (NeurIPS 2024, arXiv:2405.17399, Abacus Embeddings)**: the weakness of transformers in arithmetic derives largely from being unable to track the exact position of each digit within a large string of digits.
- **Singh & Strouse (arXiv:2402.14903)**: model errors under standard tokenization are stereotyped — "systematic rather than approximate." With right-to-left (comma-style) tokenization, GPT-4's addition rose from 84.4% to 98.9%.
- **Levy & Geva (NAACL 2025, arXiv:2410.11781)**: Llama 3 / Mistral 7B represent numbers digit by digit, and errors are close in digit space, not in value space.
- **Mohsenvand (Sketchpad, arXiv:2611.04434)**: the carry is literally a 1-bit state (blank or one), and losing it is the classic failure of long-digit calculation. The countermeasure is to externalize that bit onto paper (a sheet), process one column at a time, and verify each stage — the same form as our frame, ledger, and piece collation.
- **Shintani (arXiv:2603.26828)**: arithmetic failure decomposes into stages (layout → the meaning of the carry → recomposition → the tens remainder). The hundreds position behaves as a carry flag.
- **Bie et al. (AI-rithmetic, arXiv:2602.10416)**: all frontier models degrade on long addition, and misalignment and errors near carries account for the majority of errors (62.9% for GPT-5, 87.9% for Claude Opus 4.1, 92.4% for Gemini 2.5 Pro). This agrees with the two seats in our Gemma measurements (misalignment and carry breakdown). However, that paper reads carry errors as independent random failures, which is in tension with our observation of reproducibility (held as an unexamined branch).
- **Sclar et al. (ICLR 2024)**: sensitivity to format — small differences in format that do not change meaning move performance greatly.
- **Tam et al. (EMNLP 2024)**: an instruction is not a guarantee. An effective constraint exists only at decode time.
- **Lookahead Limitation (BlackboxNLP 2025)**: LLMs use a one-digit lookahead heuristic for carries and break down on multi-term addition.
- **Literature cited by GPT**: OpenAI, "Why Language Models Hallucinate" (2025); Farquhar et al., semantic entropy (Nature, 2024); Microsoft Research, the "Eureka" report; a NeurIPS 2025 paper on reproducibility and GPU computation conditions (as presented by GPT; the title has not been collated).
- **Our adjacent survey**: in the similarity survey on continuity-readability misrecognition (2026-11-27), no established equivalent was found for the layout claim, the name, the general anchor rule, or the .0 method (research_notes/llm-digit-run-error-similar-work-20261126-2143/report.md).

### 8.4 Our examination — our own points of arrival
The following is not citation. It is our own examination, obtained from the measurements of 2026-11-27 and the user's formulations.
1. **Errors are reproductions determined by conditions**: an error against the same text reproduces character for character (the two identical outputs of the first half of #12). If it were probabilistic fluctuation, reproduction of the identical error would be unlikely to occur. Errors are structure, and the ground on which condition search and the tuning of inference stand is here.
2. **Tuning of inference**: with the vessel (the weights) unchanged, the results move merely by changing the structure of the inference — where inference is permitted, where it is replaced by form, and where checks are placed (0/4 → 4/4). The object of tuning is not the LLM but the inference.
3. **Substrate and world**: the combination of conditions and software (and the hardware path) is the substrate, and it determines the world. Ima are determined for each world. When a countermeasure makes an error appear to disappear, the world has switched; the ima of the original world remain in the original world's table.
4. **The mechanism of appearance**: limit → completion → decision to 1 or 0. The position of the limit is determined by the world; which side it leans to is determined by the vessel's habits. Because both are fixed, the same answer comes out. The true identity of "fluctuation" is this decision.
5. **A strict definition of identity**: "the same LLM" means that the version, the size, and the training are the same. If those three are fixed, the face of the die is always the same. The language of probability expresses the observing side's ignorance before measurement. Note that identical hardware does not exist — hardware is part of the substrate, and an imum table attaches to each world, not to a model name.
6. **Separation of layers**: the world constituted by the definitions of 0 and 1 (the layer of signs and judgments) is the layer of definitions, and is not subsumed within the system of addition. GPT tries to subsume it on the side of addition, but that is to be separated. Judgment is the work of the layer of definitions.
7. **Scope**: this Part is composed only within the digital world. There are no imaginary numbers in the digital world. Subdivision bottoms out at the bit — on the side of formulas and inference there is a below, but on the physical side there is not. This is the LLM's indivisible wall, and the position of the wall can be measured only from outside the inference (on the side of record and collation).
8. **A view of operations**: composition is an accumulation of additions and subtractions, and multiplication and division are also such accumulations. Operations that require width of state-holding, such as the borrow, hit the limits of the circuit and the memory area, and err more. If multiplication is put into the question, compression erases the addresses of the seats, and errors increase. The method of the imum table requires that the accumulation be held in expanded form.
9. **The structure of the problem**: a problem that contains bit movement produces position shifts and carry breakdowns at the same seats even for unused inputs. What an imum table records is not cases but the structure of the world. Therefore prediction for unused inputs is not generalization from memorization (an answer to GPT's certification condition).
10. **Trunk and branches**: what has been admitted, and the definitions, are fixed (the trunk). Counterarguments are not closed (the branches) — that branches remain is the essence of inference, and a counterargument is not a cancellation of an agreement but a question that opens at a new address.
11. **The number of appearances**: if the size and the scale of the split are fixed, the number of appearances of ima is fixed at a constant number. That is all. Appearance is not probability; it is a function of the size of the world and the way it is cut.
12. **Parallel processing and granularity (the hardware layer)**: whether NVIDIA or ARM or anything else, misses in parallel processing cannot be prevented. It is because, as the granularity of information becomes finer, there are ima that necessarily occur. Prevention is in principle not within our hands; all that can be done is detection, and attaching addresses and designing around them — this is the same at the layer of inference and at the layer of hardware.

### 8.5 Evidence — tests and verifications (2026-11-27, Gemma 4 31B, Duck.ai guest)
The task was sign-pattern calculation on a 7×7 matrix (64 items; the key is held in master-side code). All values were collated against the key. Detailed record: test-env/gemma-adopted-form-run-2026-11-27.md.
- **16-split run**: contributions matched 48/64. The .0 form was maintained in all items. Failures closed within pieces (whole-piece collapses were #25–#28 and #41–#44), and the residuals could be identified by position. The 4-split stalled on generation volume and did not complete the run (the limit of the vessel's working area).
- **v2 piece 01**: in the form that hands δ over by number + rule, the applied δ was 1/4 (the others used the δ of #5, #3, and #7). In piece 02 (the form that places the vessel's own verified #1 in the frame), the applied δ was 4/4 — behavior appeared in which it wrote out the conversion and corrected itself. The 3 residuals of piece 02 (#5, #6, #8) can all be explained by a single substitution: the 6th term of s_5, a_65 = 4.0, was processed as 2.0 (under the assumption of processing two at a time, the value of the 2nd seat of pair 1 enters the 2nd seat of pair 3 — a 1-bit slip of the pair number, 3 → 1). The user's judgment: s_5 is a 1-bit overflow.
- **v3 separation of variables**: in the first run, which placed a 64-cell null matrix at the head, #1 itself collapsed (contributions 0/4). When it was returned to a single-slot frame, #1 matched at all layers even with integer input, and failure remained only in the δ layer (applied δ 1/4). The conclusions of the separation: (a) the shape of the frame breaks #1; (b) the form of the input (.0 or integer) moves the layer of δ conversion; (c) the form of the report does not reach the calculation and decides only whether a discrepancy appears.
- **v4 piece 02**: in the indirect-reference form (no δ field), the applied δ was 0/4 (as #7→#43, #5→#49, #8→#57, #6→#53, each became a computation of a different pattern that was internally fully consistent). Double diagnosis with GPT: agreement that what was lost was the workspace of the conversion (the externalized checkpoint that detects and corrects derivation errors). GPT attached a reservation about confounding (the patterns and the state of the frame also differ) and proposed Plan A (restoring the conversion check).
- **v5 Form B (Plan A + the fixed format of the check line)**: piece 02 went from Form A 0/4 to Form B 4/4 (same patterns, same frame, same order). Piece 01 was also 4/4. By putting the check line (number, bit string, and the weighted sum-back) into a fixed one-line format, the check itself can be collated by the master's code, and the seat of an error can be mechanically separated in the three directions of conversion / application / calculation.
- **8-split (8 patterns per piece, Form B)**: piece 1 was 8/8. Piece 2 was 6/8 — #12 and #16 dropped only the last term of s_5 (reading a_75 = 3.0 as 1.0), and the occurrences were at positions 7 and 8 within the piece. The user's design decision: the basic principle is to compose so that it does not overflow; only the end is split finely; the assessment is that 8 patterns per piece is borderline.
- **The front-half form of tail refinement**: the front half of #12 reproduced the identical result character for character, both in a parallel throw and in a solo re-throw (the check line was correct; at s_1 the application of δ6 inverted, and generation stopped immediately after). The influence of the parallel throw was negated, and it was established as a reproducible response to that text. In the front half of #16, even though the check line was correct (bits 001111), the applied δ became 011111 — an instance in which the three-direction separation functioned as a diagnosis. In GPT's verification, there was agreement that the seat was not the size of the split but the change of the front-half form, that the three-direction separation functioned, and on the design judgment of not splitting inside a pattern but reducing the number of patterns in the pieces on the tail side.
- **The prohibition-clause control**: in a control throw that removed only the one line of the prohibition clause from the #16 front-half form, the error in the applied δ (011111) reproduced identically. By GPT's decision table, it was separated out that the prohibition clause is not the cause of the application slip.
- **The position-label control**: in the version of 8-split piece 2 with position labels (i1)–(i7) attached to the terms of s_5, the targets #12 and #16 were resolved, and the term readings of the labeled column were all correct in the 8 items. The residuals moved to the next seat (the sum): in s_2 of #9 and s_5 of #10, although all the terms were correct, the sums were reported too small (12 and 14 against 16). A countermeasure works at its seat, and the overflow appears at the next seat — a measurement that ima are objects not of removal but of addressing.
- **GPT's judgment (2026-11-27)**: it supports the interpretation "not fluctuation but decision" as a strong explanation based on measurement. The components (deterministic decoding, deterministic classifiers, systematic error) are known, but it answered that the fixed 1-bit slips reached by subdivision, their reproducibility, and the conception of advance prediction by an imum table were not known to it as established theory. The reservations are two: that the explanation of the mechanism is at this point a hypothesis, and that certification of prediction requires confirmation on unused inputs. The user's answers to these are in items 5 and 9 of 8.4.
- **Where the verification resides**: the full run record, test-env/gemma-adopted-form-run-2026-11-27.md; the record of the day, memory/2026-11-27.md; the collation chats with GPT (literature countermeasures 6b09012e, judgment and knowledge answer 6b09134e, trunk and branches 6b091583).

### 8.6 Reservations and undetermined branches
- The size of the minimum unit (whether it really is 1 bit) is undetermined (8.2). What is established is fixedness.
- The explanation of the mechanism (that reaching the limit produces a binary choice) is placed in the layer of hypothesis (GPT's reservation). It is to be separated from the layer of the phenomenon (reproducibility and fixedness).
- The tension between the literature side's reading that "carry errors are random" (Bie et al.) and our observation of reproducibility is held as an unexamined branch.
- Many of the claims of this Part include screening at n=1. Certification of prediction by an imum table (hits on unused inputs) is a subject of future verification.

### Appendix A — GPT's opinions (reference, 2026-11-27)

This appendix attaches GPT's verification opinions as reference, and does not incorporate them into the trunk of this Part (the definitions and agreements). The theory of ima is not yet complete. GPT's opinions are quite important — in particular, its pointing out of the places where our compilation had dropped GPT's own opinions, and the distinction of units, become collation points for future work.
Source: chat https://chatgpt.com/c/6b091883-8ae4-83ee-8b74-5ea77c7ac777 (both the full text of the verification report and GPT's own shortened version have been confirmed).

What was thrown: the compiled text (the theoretical parts of Part 8: 8.0 / 8.1 / 8.2 / the gist of 8.4 / 8.6), and the two items recorded as GPT's opinions (Opinion 1 = the three meanings of 1 bit; Opinion 2 = the judgment and knowledge answer on "not fluctuation but decision"). GPT also stated that it had created a Word document, imum_part8_verification_report.docx.

## GPT's conclusions (gist)
- It evaluates the way of putting things together as "imum" positively as a concept. There is no fatal internal contradiction that would collapse the theory.
- Inheritance of Opinion 2: basically correct. However, the "decision it makes itself" of 8.1 is ontologically stronger than GPT's "supports it as a strong explanation." The mechanism is in the layer of hypothesis (consistent with 8.6).
- Inheritance of Opinion 1: there are important omissions. 8.2 kept the "minimum unit of information" and fixedness, but three points that were the center of GPT's opinion have dropped out: (a) 1 bit is the minimum unit of a judgment that can be fixed with responsibility; (b) 1 bit is the minimum unit of verifiability; (c) fixedness is not proof of correctness, and correctness requires another judgment, another viewpoint, external collation. The necessity of external collation should be kept in the theoretical part.
- 8.0, "there is no fluctuation": the range of the assertion is too wide. It proposes limiting it to "for the fixed errors observed this time, the explanation of fluctuation is not adopted."
- 8.1, "if there is an imum table, the numbers that will be gotten wrong are known in advance": an unverified predictive proposition. Recording for known inputs and prediction for unused inputs are two stages. At the present stage, "can possibly be predicted" is appropriate.
- The items of 8.4: (1) support, (2) support, (3) interpretation, (4) hypothesis, (5) needs revision — the identity of the model and the reproduction conditions of the experiment are separate (the input text, decoding, quantization, runtime, and computation path also have effect). Separate "the identity of the model" from "the identity of the experimental world in which ima hold." The meaning of "identical hardware does not exist" also needs clarification. (6) undefined, (7) needs distinction — the tension with 8.2 is resolved if "the minimum unit of representation = bit" and "the empirical unit of ima = undetermined" are separated, and this distinction is very important. (8) needs verification, (9) prediction hypothesis, (10) consistent, (11) unverified — a rule or function that derives the number of appearances is needed; treat it as a research proposition.
- The three points of organization: 8.0 and 8.6 (limiting the range of observation), 8.1 and 8.6 (separating observation and inference), 8.2 and 8.4(7) (separating the unit of representation and the empirical unit).
- Three points it particularly evaluates: the idea of the address; the separation of vessel and world; trunk and branches.
- GPT's proposal: divide the propositions into five layers (observed facts / definitions and addresses / the hypothesis of the mechanism / prediction by the imum table / research propositions such as the number of appearances). The most important experiment is one: with an input not used in making the imum table, predict the positions where errors will occur from the table alone.
- The conclusion in one sentence: the imum has the possibility of advancing from a concept that records errors to a concept that records the structure of error occurrence and predicts future errors. However, that predictive ability is precisely the core that must be demonstrated next.
- On the name: there is no need to require in advance that it be identical to a mathematical prime. What matters is whether the defined object has reproducibility and can withstand independent verification and prediction.

#### GPT's own shortened version (original text)
- Opinion 1 (minimum unit): partially inherited. The relation between fixedness and 1 bit is treated, but important parts are missing: "the minimum unit of judgment," "the minimum unit of verifiability," and "correctness requires external collation."
- Opinion 2 (determinism): basically inherited correctly. Reproducibility, the explanation of a fixed decision tendency, the hypothetical nature of the mechanism, and the necessity of predictive verification on unused inputs are carried over.
- Points of concern: there is no major theoretical breakdown. (1) Limit "there is no fluctuation" to the range of this measurement. (2) Treat "limit → completion → decision" as a mechanism hypothesis. (3) Advance prediction by an imum table requires verification on unused inputs. (4) Distinguish the identity of the model from the identity of the experimental conditions. (5) Distinguish bit as the minimum unit of representation from the empirical minimum unit of ima. (6) The proposition that the number of appearances becomes constant is still unverified.
- Evaluation: positive. It evaluates that the value lies in treating errors not as individual failures but as reproducible decision positions tied to conditions, and that by the "address" it advances from recording errors to grasping the structure of their occurrence. "Fix the trunk, do not close the branches" is also appropriate.
- The next most important task: whether, with an input not used when making the imum table, the positions of error occurrence can be predicted in advance.
- Final conclusion: the core concept is promising, and this compilation is also broadly appropriate. However, observed facts, theoretical interpretation, mechanism hypotheses, and predictive propositions need to be clearly separated. The next core is the demonstration of prediction on unused inputs.
- A measurement of compression (Muse's observation): what dropped first in the shortened version was the per-item judgment table of 8.4 and GPT's five-layer proposal, while GPT's own reservations (the six points of concern) remained, numbered and almost intact.