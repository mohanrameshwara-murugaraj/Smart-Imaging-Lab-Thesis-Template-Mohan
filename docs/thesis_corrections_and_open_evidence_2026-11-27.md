# Thesis corrections and remaining primary-evidence requirements
**Date:** 2026-11-27
**Scope:** Active thesis sources on `main` (not the archived copies).
**Status:** Source-grounded revisions committed. A complete thesis build and original figure restoration remain open.

## Changes committed

### Chapter 5 results presentation
- Removed five substantially duplicated tables: original quality inventory, 2D segmentation partition, 2D classification partition, the first historical 3D timing summary, and the standalone 88-dynamic sensitivity/specificity trade-off table.
- Updated their in-text references to surviving tables in Methods or Results. Do not equate annotation counts with matched available image--mask pairs.
- Added three original, **reconstructed** PGFPlots figures directly in the Results TeX. They are computed from previously tabulated values, **not restored historical plots**:
  1. `fig:results_crop_failure_rates_reconstructed`: 2/1,258 (0.16%) unavailable Good-labelled crops, 28/271 (10.33%) unavailable Artifact-labelled crops.
  2. `fig:results_dynamic_policies_reconstructed`: sensitivity/specificity Original 16.7/94.3%; Balanced 66.7/94.3%; Recall-oriented 66.7/85.7% on 88 dynamics.
  3. `fig:results_matched_3d_2d_reconstructed`: 3D sensitivity/specificity 100/84.7%; 2D 50/93.2% on 71 matched dynamics; 3D training independence unresolved.
- Replaced two statements that misleadingly suggested original segmentation history or Dice distribution plots were available in the repository with explicit disclosure of absent figure assets.
- Results chapter now contains 26 table floats and three figure floats (previously 31 tables and no figures).

### Front matter / LaTeX
- Corrected the Abstract chapter heading and its running-header command.
- Set author, official thesis title, matriculation number, and first supervisor consistently from the existing title page in `preamble.tex` and reused macros in the title-page source.
- Changed abstract wording so low *offline* 2D latency is not presented as an independently matched end-to-end speedup or prospective scanner integration.
- Expanded the abbreviation inventory to terms used in the active thesis; removed unused FMRI and made the list display without relying on `\\ac` invocation in chapter text.
- Consolidated redundant package loads and pinned PGFPlots compatibility to version 1.18.
- **Not independently verified:** institutional logo identity, enrolment/degree wording, editing-period dates and university-specific title-page rules.

### Discussion
- Reduced repeated discussions of cohort sizes, 3D export provenance, early stopping and processing dependencies across Sections 6.6--6.8.
- Kept the principal methodological cautions in place: no verified independent 3D test, no validated live scanner intervention, no demonstrated cause-specific artifact detector from empty masks, no controlled 3D/2D speedup.

## Primary-data questions still open

| Issue | Status / next needed evidence |
|---|---|
| Original segmentation training trajectory `H01_segmentation_dice_history.pdf` | Historical full epoch plot not in repository; obtain original curve PDF or raw epoch CSV. Sparse values alone must not be presented as complete trajectory. |
| Original Dice distribution `R01_segmentation_dice_distribution.pdf` | Missing; recover per-slice Dice export or original plotted PDF. Group means/medians do not reconstruct distributions. |
| Classifier train/validation history plots | Missing or misnamed historically; obtain actual oracle and predicted run logs or original plots before figure inclusion. |
| Qualitative MRI panels `R09_revised_identified.pdf` | Missing; obtain verified MRI and masks/crops with identifiers. Do not synthesize clinical-looking examples from numeric metadata. |
| R06/R07 original score histograms and additional plots | Missing; may be restored from raw prediction scores. A reconstructed policy figure is **not** a substitute for score distributions. |
| 3D ResNet checkpoint and training/evaluation split | Unknown; no claim of independent held-out 3D classification performance unless exact checkpoint exposure is verified. |
| nnU-Net matched test metrics | Not verifiable from available artifacts; keep quantitative independent test segmentation claims unreported. |
| 2D Dice/IoU evaluator and empty-mask policy | Original evaluation implementation/output needs verification. Preserve 0.8620 Dice / 0.8339 IoU as reported; do not reconcile without raw data. |
| 2D timing `5.234 / 5.239 / 5.257 ms` | The four stage means sum to 5.234ms; two reported totals differ. Keep these values labeled with their reporting scope until the timing log is recovered. |
| Acquisition 35 versus 36 slices | Verify scanner acquisition and original image array metadata; current text appropriately distinguishes source protocol from processing records. |
| Full LaTeX/Biber build | Still required on the complete checkout: `latexmk -pdf -shell-escape -interaction=nonstopmode main.tex`, then inspect output PDF, floats, institutional logo, and warnings. PGFPlots figure constructs have been tested in isolated minimal LaTeX builds, not the complete thesis. |
| Conclusion chapter | Still contains only headings. Out of scope of this correction pass. |

## Static consistency check after editing
- Checked all active `\\label`, `\\ref`, `\\eqref`, `\\include`, `\\input`, and bibliography citation keys against available declarations: no unresolved targets.
- Verified no duplicate label definitions or unbalanced basic LaTeX environment counts.
- Inspected all referenced external graphics paths in source; existing external figures were found in the repository.
- The three new plots use embedded PGFPlots and require no extra binary figure files.
- Historical missing figures were **not** fabricated or silently claimed as restored.

## Editorial guardrails
1. Keep the 600-volume 3D classifier export separate from a verifiable independent-subject test population.
2. Keep the 1,529 full oracle test and the 1,499 successful predicted-crop test separate, with a different matched 1,499-slice analysis.
3. Keep the 86 validation, 88 test, and 71 paired-dynamic populations separate.
4. Distinguish dynamic-inherited labels from directly observed visible slice artifact ground truth.
5. Keep retrospective rule improvements separate from demonstrated scanner-time early stopping.
