# Data Usage and Ethics

## Dataset Availability

This repository does not include the NeuroMM-2026 dataset. The dataset is a controlled-access challenge dataset and must be requested from the official NeuroMM-2026 Hugging Face page.

## Files Not Included

The following files are intentionally excluded from this repository:

```text
*.tar
*.zip
*.npy
*.csv
train_data/
candidate_data/
processed/
annotations/
candidate/
neuromm26_results/
checkpoints/
submission.csv
sample_submission.zip
*.pt
*.pth
*.ckpt
```

These files may contain original data, derived data, annotations, challenge candidate data, predictions, or trained model parameters.

## Reproducibility

To reproduce the experiment, users should:

1. Request official dataset access.
2. Download the required Track 1 files:
   - `neuromm2026_train_val.csv`
   - `candidate_ids.txt`
   - training/validation EEG archive
   - candidate EEG archive
3. Rename them as:
   - `neuromm2026_train_val.csv`
   - `candidate_ids.txt`
   - `train_eeg.tar`
   - `candidate_eeg.tar`
4. Run the Colab notebook.

## Ethical Considerations

Clinical EEG data should be handled carefully. This repository only provides code for academic and challenge-related experimentation. It does not attempt to identify individuals, redistribute clinical data, or provide clinical diagnosis.

The output of this model is for challenge evaluation and research only. It should not be used as a medical diagnostic tool.
