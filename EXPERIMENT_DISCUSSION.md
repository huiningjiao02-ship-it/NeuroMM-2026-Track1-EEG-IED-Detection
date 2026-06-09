# Experiment Discussion

## 1. Task Overview

This project addresses Track 1 of the NeuroMM-2026 Challenge, which is an EEG-only binary classification task for interictal epileptiform discharge (IED) detection. Each EEG sample is represented as a 29-channel, 4-second signal segment. The objective is to classify whether a segment contains an IED/spike event.

This is not a standard balanced classification problem. IED samples are relatively rare compared with non-IED samples, which makes the model vulnerable to class-imbalance effects. Therefore, accuracy alone is not sufficient for judging model performance.

## 2. Experimental Setup

The initial experiment used the official EEG-only baseline workflow.

```text
Model: legacy/tcnet_eeg
Epochs: 30
Seed: 0
Learning rate: 5e-4
Batch size: 64
Platform: Google Colab GPU
```

The pipeline included:

- Official baseline code installation.
- EEG data extraction from `.tar` files.
- Train/validation annotation loading.
- EEG file path organization.
- Model training and validation.
- Candidate-set prediction and CodaBench submission generation.

## 3. Validation Results

| Metric | Value |
|---|---:|
| AUPRC | 0.7288 |
| Binary-F1 | 0.6615 |
| Macro-F1 | 0.8118 |
| Weighted-F1 | 0.9332 |
| Accuracy | 0.9317 |
| Balanced Accuracy | 0.8260 |

## 4. Result Interpretation

The validation accuracy is high, but it should be interpreted carefully. In a class-imbalanced IED detection task, a model may achieve high accuracy by correctly predicting the majority non-IED class while still missing a meaningful number of positive IED cases.

For this reason, the most important metric is AUPRC. AUPRC evaluates how well the model retrieves positive IED samples across different decision thresholds and is more informative when the positive class is rare.

The initial AUPRC of 0.7288 suggests that the pipeline is able to learn useful EEG patterns related to IED events. The balanced accuracy of 0.8260 further indicates that the model is not relying only on the majority class.

## 5. Strengths of the Experiment

### Reproducible workflow

The project was organized as a Google Colab pipeline, making the setup and execution easier to reproduce. The workflow includes installation, data preparation, training, evaluation, and submission generation.

### Challenge-aligned evaluation

The experiment uses AUPRC as the key metric, which is consistent with the clinical EEG detection setting and the class imbalance of the task.

### Practical submission pipeline

The code generates `sample_submission.zip` for CodaBench, which makes the project not only a local validation experiment but also a complete challenge-submission workflow.

### Data integrity checks

The code checks whether EEG `.npy` files can be found after extraction and verifies the shape of loaded EEG samples. This reduces the risk of silent data-path errors.

## 6. Limitations

### Baseline model only

The current experiment uses `legacy/tcnet_eeg`, which is a relatively lightweight temporal convolutional model. It is suitable for quickly validating the pipeline, but it may not be the strongest architecture for the final leaderboard.

### Single-seed result

The current result is based on one random seed. For a more reliable comparison, future experiments should report mean and standard deviation across multiple seeds.

### Limited model comparison

The experiment has not yet compared stronger baselines such as `legacy/densenet121_eeg`, `legacy/resnet50_eeg`, or multi-model ensemble methods.

### No explicit imbalance optimization yet

Although AUPRC is reported, the training loss has not yet been modified with class weighting, focal loss, or positive-sample oversampling. These may improve sensitivity to IED samples.

### Limited interpretability

The current project does not yet include channel-level or time-level interpretability analysis, such as occlusion sensitivity, saliency maps, or attention-like visualization.

## 7. Possible Improvements

Future work can improve the project in several directions:

1. Train stronger EEG backbones such as `legacy/densenet121_eeg` or `legacy/resnet50_eeg`.
2. Use class-weighted loss or focal loss to address IED class imbalance.
3. Add EEG data augmentation, such as Gaussian noise, time shifting, channel dropout, or mixup.
4. Run multiple seeds and report mean ± standard deviation.
5. Build an ensemble of several high-performing models.
6. Add interpretability analysis to identify informative EEG channels and time windows.
7. Extend the pipeline to Track 2 and Track 3 by incorporating official video features.

## 8. Final Summary

The current experiment successfully builds and validates a complete EEG-only IED detection pipeline for NeuroMM-2026 Track 1. The result is suitable as a first valid challenge submission and as a research project entry in a CV. However, for stronger competition performance, additional model comparison, imbalance-aware learning, multi-seed validation, and ensemble strategies should be explored.
