RARE26 PHASE 2 FINAL PACKAGE README

PHASE 2 STATUS
Phase 2A: MaxViT-Tiny 384 pilot and hybrid ensemble completed.
Phase 2B: Bootstrap confidence intervals and per-centre robustness completed.
Phase 2C: Threshold sensitivity analysis completed.
Phase 2D: Final model locking and master thesis summary completed.
Phase 2E: Final report package and combined thesis conclusion completed.

FINAL LOCKED MODEL
Model name: Phase 2A Selected Hybrid Ensemble

Model components:
1. Baseline ResNet-18, weight 0.90
2. Weighted-Augmentation ResNet-18, weight 0.05
3. MaxViT-Tiny 384, weight 0.05

Final threshold:
0.027517

Threshold source:
Validation-selected threshold only.
Held-out test set was not used for final threshold selection.

FINAL HELD-OUT TEST PERFORMANCE
Recall / Sensitivity: 0.9615
PPV / Precision: 0.3205
Specificity: 0.8801
AUROC: 0.9888
AUPRC: 0.9154
False Positives: 53
False Negatives: 1

MAIN IMPROVEMENT OVER PHASE 1
Phase 1 false positives: 86
Final hybrid false positives: 53
False-positive reduction: 33 cases
Recall remained unchanged.
False negatives remained unchanged.

BOOTSTRAP RELIABILITY
Mean false-positive reduction: 32.57
95% CI: [21.00, 45.00]

Mean PPV gain: 0.0954
95% CI: [0.0565, 0.1417]

Mean specificity gain: 0.0737
95% CI: [0.0475, 0.1018]

PER-CENTRE ROBUSTNESS
center_1 false-positive reduction mean: 22.18
95% CI: [13.00, 33.00]

center_2 false-positive reduction mean: 11.04
95% CI: [3.00, 19.00]

THRESHOLD SENSITIVITY
The final threshold remains 0.027517 because it was validation-selected.
The test threshold sweep was used only for sensitivity analysis.
A higher test-sweep threshold of 0.039 showed PPV 0.4167, recall 0.9615, FP 35, FN 1, but it was not adopted because it was identified using held-out test analysis.

IMPORTANT FINAL FILES
1. Phase 2A final summary:
results/phase2a_final_clean_summary_table.csv

2. Phase 2B bootstrap CI:
results/phase2b/phase2b_cell8_bootstrap_ci_summary.csv

3. Phase 2B improvement CI:
results/phase2b/phase2b_cell8_improvement_ci_summary.csv

4. Phase 2C threshold sensitivity:
results/phase2c/phase2c_threshold_sensitivity_sweep.csv

5. Phase 2D final model lock manifest:
results/phase2d/phase2d_final_model_lock_manifest.csv

6. Phase 2D final operating point:
results/phase2d/phase2d_final_operating_point.csv

7. Phase 2D master thesis table:
results/phase2d/phase2d_final_master_thesis_summary_table.csv

8. Phase 2E package manifest:
results/phase2e/phase2e_final_package_manifest.csv

9. Phase 2E final combined thesis conclusion:
results/phase2e/phase2e_final_combined_thesis_conclusion.txt

FINAL INTERPRETATION
The Phase 2A Selected Hybrid Ensemble is locked as the final thesis model. It improves PPV, specificity, and false-positive burden compared with the Phase 1 ensemble while maintaining high recall and the same false-negative count. This makes it the strongest current model candidate for high-sensitivity early neoplasia screening in Barrett’s esophagus endoscopy.