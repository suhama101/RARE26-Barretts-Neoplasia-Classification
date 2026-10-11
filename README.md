# RARE26 Barrett's Neoplasia Classification

This repository contains notebooks for a RARE26 (EndoVis, MICCAI 2026) study of binary classification of Barrett's esophagus endoscopy images: non-dysplastic Barrett's esophagus (NDBE) versus early neoplasia. The work focuses on a low-prevalence setting, where missed neoplasia and false-positive referrals both matter. The repository contains the analysis notebooks; the image data, split manifests, trained checkpoints, and Docker handoff are not included in this checkout.

## Clinical problem

Barrett's esophagus surveillance contains many NDBE images and relatively few images showing early neoplasia. This class imbalance means that accuracy alone is not a sufficient description of performance: a model can appear accurate while missing neoplasia, and lowering the referral threshold can increase recall while also increasing false positives. The experiments therefore considered recall, PPV, specificity, AUROC, AUPRC, calibration, referral by uncertainty, centre-wise evaluation, and error or artifact review.

## Dataset and splits

The project dataset contains 3,088 unique images. The split sizes are:

| Split | Images |
|---|---:|
| Train | 2,159 |
| Validation | 461 |
| Local test | 468 |

The local test set contains 26 neoplasia images. The notebooks read the image archive and split manifests from external Google Drive or Colab paths; those data files are not stored in this repository.

## Method

### Phase 1: ImageNet ResNet-18 baselines

The baseline notebook uses an ImageNet-pretrained ResNet-18 with a two-class output. The comparisons covered class-weighted cross-entropy, focal loss, balanced sampling, augmentation, centre-wise and cross-centre evaluation, a validation-tuned ensemble, temperature scaling, uncertainty-based referral, and an error/artifact audit. In the reported comparisons, weighted loss with mild augmentation worked best; focal loss and balanced sampling did not improve the results.

### Phase 2: MaxViT-Tiny hybrid

MaxViT-Tiny at 384 was added as a second model family. Standalone MaxViT was not better in the reported comparison. A small MaxViT contribution to a hybrid ensemble reduced false positives in bootstrap-checked and per-centre checks.

### GastroNet extension

The extension used released GastroNet-5M DINOv1 pretrained checkpoints. It did not pretrain a foundation model from scratch on the full RARE26 dataset. RN50 and other checkpoint variants were evaluated with three-fold cross-validation, paired comparisons, and seed-stability checks. One checkpoint was unstable on the original validation split.

### Final ViT-S ensemble

The final model is a `timm` `vit_small_patch16_224` with a one-logit head, initialized from GastroNet-5M DINOv1. It averages probabilities from 3 seeds: 42, 2026, and 3407. The threshold was locked at `0.23794867920676918`, selected on validation without post-test tuning. Preprocessing is RGB conversion, bicubic resize to `224x224`, and ImageNet mean/std normalization.

| Model or analysis stage | Configuration | Reported outcome |
|---|---|---|
| ResNet-18 baseline | ImageNet initialization | Baseline family for the Phase 1 comparisons |
| MaxViT-Tiny | Input size 384 | Standalone model was not better; a small hybrid weight reduced false positives |
| GastroNet extension | Released GastroNet-5M DINOv1 checkpoints; three-fold CV | Checkpoint variants were compared; one was unstable on the original validation split |
| Final ViT-S ensemble | `vit_small_patch16_224`; 3 seeds; locked validation threshold | Final local held-out evaluation reported below |

## Final local held-out result

This is a single evaluation on the local held-out test split. The local test was also used in earlier analyses, so these numbers should be interpreted conservatively.

| Metric | Value |
|---|---:|
| Recall | 0.9615 (25/26) |
| PPV | 0.6579 |
| Specificity | 0.9706 |
| AUROC | 0.9892 |
| AUPRC | 0.9614 |
| TP | 25 |
| TN | 429 |
| FP | 13 |
| FN | 1 |

## Key findings

- Weighted loss with mild augmentation was the strongest Phase 1 baseline configuration in the reported comparisons.
- Focal loss and balanced sampling did not help in those comparisons.
- Standalone MaxViT-Tiny was not better than the comparison baseline, while a small MaxViT weight in a hybrid ensemble reduced false positives in the reported checks.
- Released GastroNet-5M DINOv1 checkpoints provided the initialization used for the final ViT-S ensemble; this was not new foundation-model pretraining.
- The final local held-out evaluation had high recall and specificity at the locked validation-selected threshold, but it contains only 26 neoplasia images.

## Limitations

- There are only 26 neoplasia images in the local test set, so recall is uncertain.
- The split is image-level and does not use patient IDs.
- Centre or acquisition shortcuts may contribute to the observed performance.
- The work used released GastroNet weights and did not perform new foundation-model pretraining.
- The local test set was used in earlier analyses, so claims are conservative.
- No hidden-test score is reported.
- There is no clinical or prospective validation.

## Repository structure

Only the following files are present in this repository snapshot:

```text
.
├── RARE26_Baseline (1).ipynb
├── RARE26_GastroNet_Experiment2_SWSL_DINOv1.ipynb
└── RARE26_Phase2A_MaxViT_Hybrid_Final (1).ipynb
```

The repository does not contain the raw images, split CSV files, model checkpoints, Dockerfile, container configuration, or a standalone inference script.

## Docker inference

Docker inference is not runnable from this repository snapshot because the Docker handoff files and inference entry point are not present here. The project handoff describes an offline `linux/amd64` container with a pinned base-image digest and pinned dependencies, checkpoint SHA-256 verification, and raw ensemble-probability output with no threshold applied. Those commands are intentionally not reproduced here because the corresponding files are absent from this checkout.

## Acknowledgements

Thanks to the RARE26 organisers for the challenge and to the GastroNet authors for releasing the GastroNet-5M DINOv1 checkpoints used in this work.
