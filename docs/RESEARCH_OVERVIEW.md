# RARE26 — Barrett's Neoplasia Classification

Medical AI experiments for early neoplasia detection in Barrett's
esophagus endoscopy images, developed in connection with the
MICCAI 2026 RARE26 EndoVis challenge.

## Research Objective

Investigate high-sensitivity neoplasia screening using CNNs,
transformer architectures, medical-domain pretraining, and ensembles,
while evaluating false positives, precision, centre-level robustness,
and threshold stability.

## Experimental Work

### Track A: GastroNet and DINOv1

- ResNet-50 with GastroNet-5M DINOv1 pretraining
- Comparison with SWSL + GastroNet-5M DINOv1
- Three-fold cross-validation on 2,620 development cases
- Paired comparisons with ViT-S
- Multi-seed stability analysis and equal-weight ensembling
- Validation-locked thresholds before held-out testing

**Final ViT-S three-seed ensemble (held-out test, n=468):**

| Metric | Value |
|---|---:|
| Recall | 0.9615 |
| Precision (PPV) | 0.6579 |
| Specificity | 0.9706 |
| AUROC | 0.9892 |
| AUPRC | 0.9614 |
| False positives | 13 |
| False negatives | 1 |

The ensemble uses seeds 42, 2026 and 3407, with equal weights.
The decision threshold was selected using validation data.

### Track B: ResNet and MaxViT Hybrid

- ResNet-18 baseline and weighted-augmentation experiments
- MaxViT-Tiny 384 investigation
- Hybrid ensemble with weights 0.90 / 0.05 / 0.05
- Bootstrap confidence intervals and centre-level analysis
- Validation-selected threshold: 0.027517

**Selected Phase 2 hybrid (reported held-out evaluation):**

| Metric | Value |
|---|---:|
| Recall | 0.9615 |
| Precision (PPV) | 0.3205 |
| Specificity | 0.8801 |
| AUROC | 0.9888 |
| AUPRC | 0.9154 |
| False positives | 53 |
| False negatives | 1 |

Compared with its Phase 1 ResNet ensemble, the hybrid reduced
false positives from 86 to 53 while maintaining recall.

## Notebooks

- `notebooks/RARE26_Baseline.ipynb`
- `notebooks/GastroNet_Experiment1.ipynb`
- `notebooks/GastroNet_Experiment2.ipynb`
- `notebooks/RARE26_Phase2_MaxViT.ipynb`

## Research Reports

Selected validation summaries, controlled comparisons, locked-test
results, and Phase 2 conclusions are included in `research_reports/`.

## Reproducibility and Limitations

- Results above are taken from saved experiment reports.
- The two experimental tracks are presented separately.
- Direct head-to-head superiority is not established here.
- Threshold selection was performed using development/validation data.
- The repository does not distribute the underlying medical dataset
  or large model checkpoints.
- Notebooks may require the original data, pretrained weights,
  environment setup, and path configuration to execute.
- These are research evaluations, not clinical validation or
  claims of official challenge leaderboard performance.

## Author

Suhama Mustafa
MS Software Engineering
National University of Sciences and Technology (NUST), Pakistan
