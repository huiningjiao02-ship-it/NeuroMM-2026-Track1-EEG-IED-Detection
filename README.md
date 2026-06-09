# NeuroMM-2026 Track 1: EEG-only IED Detection

This repository contains my reproducible Google Colab pipeline for **Track 1 / NMM-Basic-IED** of the NeuroMM-2026 Challenge. The task is EEG-only binary detection of interictal epileptiform discharges (IEDs), also referred to as spike vs. non-spike classification.

## Project Summary

The project focuses on building a clean and reproducible EEG-based deep learning workflow for clinical EEG event detection. It uses the official NeuroMM-2026 baseline code and organizes the process into five steps:

1. Environment setup and baseline installation.
2. Data preparation and EEG file extraction.
3. GPU and data integrity check.
4. Track 1 EEG-only model training.
5. CodaBench submission file generation.

## Current Validation Result

The initial experiment used the official EEG-only baseline model:

```text
Model: legacy/tcnet_eeg
Epochs: 30
Seed: 0
Learning rate: 5e-4
Batch size: 64
```

Validation results:

| Metric | Value |
|---|---:|
| AUPRC | 0.7288 |
| Binary-F1 | 0.6615 |
| Macro-F1 | 0.8118 |
| Weighted-F1 | 0.9332 |
| Accuracy | 0.9317 |
| Balanced Accuracy | 0.8260 |

Because the IED detection task is class-imbalanced, **AUPRC** and **balanced accuracy** are more meaningful than raw accuracy.

## Repository Contents

```text
README.md
CV_Entry_NeuroMM2026.md
EXPERIMENT_DISCUSSION.md
DATA_USAGE_AND_ETHICS.md
NeuroMM2026_Track1_Colab.ipynb
track1_colab_pipeline.py
requirements.txt
.gitignore
LICENSE
```

## Data

The NeuroMM-2026 dataset is not included in this repository. Users must request access from the official NeuroMM-2026 Hugging Face dataset page and follow the challenge data-use agreement.

The expected local files for Track 1 are:

```text
neuromm2026_train_val.csv
candidate_ids.txt
train_eeg.tar
candidate_eeg.tar
```

The code supports both the official `.tar` files and nested `.tar` files in case the files were accidentally re-packed before uploading.

## How to Run

The recommended way is to open `NeuroMM2026_Track1_Colab.ipynb` in Google Colab and run the cells from top to bottom.

Before running:

1. Set Colab runtime to GPU.
2. Upload the required data files to Google Drive.
3. Make sure file names match:
   - `neuromm2026_train_val.csv`
   - `candidate_ids.txt`
   - `train_eeg.tar`
   - `candidate_eeg.tar`

After running the final cell, the notebook generates:

```text
sample_submission.zip
```

This file can be uploaded to CodaBench for Track 1.

## Notes

This repository is for research and challenge participation only. It does not redistribute the NeuroMM-2026 dataset, annotations, candidate set, model checkpoints, submission files, or derived data.
