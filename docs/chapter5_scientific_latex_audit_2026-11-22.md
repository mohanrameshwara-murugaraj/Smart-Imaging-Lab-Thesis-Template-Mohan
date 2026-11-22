# Chapter 5 — Scientific and LaTeX Consistency Audit
**Date:** 2026-11-22  
**Scope:** Active `content/4_result.tex`, checked against `content/3_method.tex`, `main.tex`, `preamble.tex`, archived `content/archive/4_result_before_restructure_2026-11-21.tex`, and the other main chapter files for cross-reference consistency.  
**Status:** Editorial and source-level audit completed; full LaTeX compilation and visual page inspection NOT executed.

## Overall outcome

Chapter 5 is structurally complete (10 top-level sections, 37 subsections) and internally consistent at the level of the major reported confusion matrices. It is **not yet suitable for final submission without figure restoration, controlled PDF build, and confirmation of unresolved experimental provenance**.

The approved 5.10 summary was committed separately as `2674970fd1c939bd05451049c734712f0e46d7a5`. This audit document records findings and does not modify the approved manuscript.

## 1. Structural and cross-reference checks — PASS, with build caveat

- All requested Sections 5.1–5.10 exist in the correct order.
- All requested subsections exist: 3 + 3 + 5 + 3 + 6 + 4 + 5 + 4 + 4 + 0 = **37**.
- The active results chapter contains **31 table floats and 0 figure floats**.
- No duplicate `\label{...}` declarations were detected across the eight `content/` chapter files included by `main.tex`.
- No unresolved `\ref{...}` or `\eqref{...}` targets were detected across these eight chapters.
- Chapter 5's reference to Chapter 4's `subsec:initial_3d_unet` resolves.
- Braces and `table`/`tabular` environment counts balanced; all 31 tabular environments passed basic row-column-count checks.
- These static checks **do not prove successful LaTeX compilation**. A full `latexmk` run and visual PDF review are still necessary.

**Numbering note:** `preamble.tex` contains `\counterwithout{table}{chapter}` and `\renewcommand{\thetable}{\arabic{table}}`, so table numbers are global rather than chapter-prefixed. This is not currently a hardcoded-reference problem: the active Chapter 5 uses `\ref` for its table references. If the faculty requires tables named 5.1, 5.2, etc., the preamble's numbering design must be revised consistently for the entire thesis.

## 2. Numerical consistency — PASS for major confusion matrices

The following reported confusion-matrix totals and derived accuracy, sensitivity, specificity, precision, and F1 were independently recomputed from the recorded `TN/FP/FN/TP` values and agreed to the stated rounding:

| Cohort | N | TN / FP / FN / TP | Recomputed interpretation |
|---|---:|---|---|
| 3D ResNet full prediction export | 600 | 469 / 39 / 0 / 92 | 93.5% accuracy; 100% sensitivity, **not independently held out** |
| 2D oracle crop, full test | 1,529 | 1,144 / 114 / 200 / 71 | 79.5% accuracy; 26.2% Artifact sensitivity |
| 2D predicted crop, usable test | 1,499 | 1,122 / 134 / 186 / 57 | 78.7% accuracy; 23.5% sensitivity |
| Matched oracle crop | 1,499 | 1,142 / 114 / 196 / 47 | 79.3% accuracy; 19.3% sensitivity |
| Original dynamic rule | 88 | 66 / 4 / 15 / 3 | 78.4% accuracy; 16.7% sensitivity |
| Balanced dynamic rule | 88 | 66 / 4 / 6 / 12 | 88.6% accuracy; 66.7% sensitivity |
| Recall-oriented rule | 88 | 60 / 10 / 6 / 12 | 81.8% accuracy; 66.7% sensitivity |
| Balanced validation | 86 | 78 / 2 / 2 / 4 | 95.3% accuracy; 66.7% sensitivity |
| Matched 3D | 71 | 50 / 9 / 0 / 12 | 87.3% accuracy; 100% sensitivity in export |
| Matched 2D | 71 | 55 / 4 / 6 / 6 | 85.9% accuracy; 50% sensitivity |

Other arithmetic checks:
- Original annotation counts: 655 Good + 115 Artifact + 66 Uncertain = **836**; 770 known-label entries.
- 2D segmentation slices: 9,264 + 3,168 + 3,168 = **15,600**.
- 2D classifier slices: 6,078 + 1,506 + 1,529 = **9,113**.
- Predicted crop availability: 5,954 + 1,498 + 1,499 = **8,951**, with 162 failures; 8,951/9,113 = **98.2%**.
- Test missing crops: 2 Good + 28 Artifact = **30**; low-Dice successful crops 21; 21 + 30 = **51**.
- Matched 3D–2D correctness: 52 both correct + 10 3D-only correct + 9 2D-only correct + 0 both wrong = **71**.
- Low-level 2D time components: 0.472 + 2.302 + 0.312 + 2.148 = **5.234 ms**.

**Unresolved timing discrepancy:** The recovered September report gives **5.239 ms** successful-slice processing and elsewhere **5.257 ms**, versus 5.234 ms from summed stage means. Chapter 5 reports this honestly instead of altering the source. Confirm the exact timing script and event boundaries before treating any single value as audited end-to-end latency.

**Metric-convention verification needed:** The aggregate 2D test Dice 0.8620 and IoU 0.8339 may employ different image/empty-mask averaging conventions. They were reported as recovered, not recomputed from per-mask inputs here. Confirm evaluator code and the treatment of empty reference/predicted masks before final submission.

## 3. Figure availability — HIGH PRIORITY

**Chapter 5 currently contains 0 active `figure` environments and 0 `\includegraphics` calls.** Seven groups of pending figure comments remain around lines 385, 449, 468, 564, 654, 698, and 760 of the current Chapter 5.

Mentioned/required visual evidence includes:
- `H01_segmentation_dice_history.pdf` (2D segmentation train/validation convergence).
- `R01_segmentation_dice_distribution.pdf` (segmentation Dice across quality categories).
- Oracle/predicted classifier training/validation histories (note archived exported `H02_segmentation_loss_history.pdf` naming conflict).
- `R02` crop availability by inherited quality label.
- `R09_revised_identified.pdf` (identified MRI/reference mask/predicted mask/crop examples).
- `R07` original dynamic confusion matrix or `R06` score distribution.
- `R08` original/balanced/recall-oriented policy comparison.
- Matched 3D/2D confusion matrix and ROC figures where original data/plots are available.

Direct requests for several of the named PDFs under the configured `pictures/` path returned 404; textual GitHub code search found manuscript mentions rather than the image assets. Thus their presence in the active TeX build **has not been verified**. This does not prove the files do not exist elsewhere. Add and verify the original figures with clear provenance and captions; do not fabricate illustrations of experiments.

The qualitative-crop narrative refers to MRI examples that the reader currently cannot inspect in Chapter 5. Make the figure available or qualify the discussion as metadata-based.

## 4. Repeated results and chapter economy — MEDIUM PRIORITY

- Section 5.4's historical 3D workflow timing table is substantially repeated in Section 5.9's 3D timing table and Section 5.9's broad timing summary. Retain the overview in 5.9 and shorten the repeated detailed table in 5.4, **or** keep the 5.4 evidence table and refer to it from 5.9.
- Section 5.7 presents original-rule counts, three-policy comparison metrics, and a sensitivity/specificity trade-off table. All are valid, but some measurements are shown repeatedly. Consolidate where practical without losing the threshold-selection context.
- Section 5.8's confusion-count, metric, agreement, and trigger-attribution tables are related but serve distinct analyses. They may be retained if the final PDF layout supports them.
- Section 5.10's compact summary table is useful if it is clearly marked as a synthesis with separate denominators.
- Thirty-one tables in one chapter justify a final pagination and float-placement review.

## 5. Alignment with Chapter 4 — SUBSTANTIALLY CONSISTENT

- 3D preliminary experiments: training/evaluation limitations and flawed Random Forest label linkage agree with the documented methodology.
- 3D nnU-Net: 600/170 volume-entry allocation and subject overlap are explicitly disclosed.
- 3D ResNet: recovered 600-record export is not incorrectly claimed to be an independently held-out subject test.
- 2D segmentation and classification: the separate 15,600-slice and 9,113-slice populations agree with Chapter 4's inventory.
- Oracle versus predicted crops: differing full-test denominators and the common 1,499-slice comparison are appropriately distinguished.
- Dynamic decisions: the 86-dynamic validation population is separated from the 88-dynamic test and 71-dynamic matched cohort.
- Failure-aware rule: missing predicted crops, not Dice computed from unavailable deployment reference masks, drive its failure indicator.
- Sequential processing: Chapter 4 describes a reconstructed-file demonstration; Chapter 5 correctly avoids claiming measured prospective scanner-time early stopping.

**Scientific confirmation still required:**
1. Independently verify whether the 3D ResNet export includes training/validation observations and reconstruct the executed checkpoint-specific split.
2. Recover a matched nnU-Net prediction/reference-mask test evaluation; otherwise keep its quantitative performance unreported.
3. Verify 2D segmentation Dice/IoU averaging and empty-mask conventions from the evaluation script.
4. Confirm source/output files and original execution provenance for matched 3D/2D CSV and the failure-aware threshold search.
5. Do not state that a failed crop is a visible artifact, or that subject-clustered acquisitions are independent.
6. Do not claim a measured scanner-to-decision latency or early-stopping speed gain without execution-linked acquisition-time records.

## 6. Remaining LaTeX submission checks — NOT YET EXECUTED

A verified `latexmk`/PDF build was **not** executed in this audit. Required next checks after figure restoration:
- Compile the entire thesis twice or via `latexmk` with bibliography processing; inspect `Undefined control sequence`, missing files, unresolved references, overfull/underfull boxes, and float warnings.
- Visually inspect all 31 Chapter 5 table placements, long `p{...}` columns, broken rows, caption readability, and page breaks.
- Verify every inserted PDF figure is included and linked to a caption describing cohort, axes, ground truth, and experimental run.
- Check front-matter/preamble placeholders separately before submission; these are outside Chapter 5's scientific audit.

## Recommendation and status

**Result:** Chapter 5 is organized and the principal tabulated confusion-matrix calculations pass. Its major unresolved risks are **figure absence, dataset/checkpoint provenance, evaluation-convention details, and lack of a validated LaTeX PDF build**. Preserve the approved chapter while resolving those issues with original evidence.

Recommended next workflow: (1) recover/verify figures and experiment logs; (2) correct only source-supported scientific inconsistencies; (3) remove redundant tables; (4) compile and visually inspect the full thesis PDF; (5) freeze the revised manuscript with a dated commit.
