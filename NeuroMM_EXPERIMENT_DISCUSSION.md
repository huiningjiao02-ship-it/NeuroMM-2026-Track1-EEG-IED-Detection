# NeuroMM-2026 Track 1 EEG-only IED Detection: Experiment Discussion

## 1. Task Overview

This project focuses on the EEG-only IED detection task in NeuroMM-2026 Challenge Track 1. The task is a binary classification problem based on electroencephalography (EEG) signals. The objective is to determine whether a given EEG segment contains an interictal epileptiform discharge (IED), also referred to as a spike event.

Each EEG sample is a 29-channel, 4-second EEG segment. The model is required to classify the EEG segment as either an IED-positive sample or a non-IED sample. Since positive IED events are usually much fewer than negative samples in clinical EEG data, the task involves a clear class-imbalance problem.

## 2. Experimental Setup

This project is based on the official NeuroMM-2026 EEG-only baseline code. The Track 1 EEG data and annotation files were used for model training, validation, and candidate test-set prediction.

The experimental setup is as follows:

| Item | Setting |
|---|---|
| Task | Track 1 EEG-only IED detection |
| Model | legacy/tcnet_eeg |
| Epochs | 30 |
| Random seed | 0 |
| Learning rate | 5e-4 |
| Batch size | 64 |
| Runtime environment | Google Colab GPU |

The overall workflow includes installing the official baseline environment, extracting EEG data files, loading training and validation annotations, organizing EEG file paths, training the EEG-only model, evaluating the model on the validation set, and generating predictions for the candidate EEG data for CodaBench submission.

## 3. Validation Results

Under the current baseline setting, the model achieved the following validation results:

| Metric | Value |
|---|---:|
| AUPRC | 0.7288 |
| Binary-F1 | 0.6615 |
| Macro-F1 | 0.8118 |
| Weighted-F1 | 0.9332 |
| Accuracy | 0.9317 |
| Balanced Accuracy | 0.8260 |

AUPRC was used as the primary evaluation metric because the task involves class imbalance. Compared with raw accuracy, AUPRC better reflects the model's ability to identify minority-class IED-positive samples.

## 4. Interpretation of Results

Although the model achieved an accuracy of 0.9317 on the validation set, high accuracy should be interpreted carefully in class-imbalanced tasks. When negative samples dominate the dataset, a model may obtain high accuracy by mainly predicting the majority class, which does not necessarily indicate that it can effectively identify IED-positive samples.

Therefore, this project places greater emphasis on AUPRC and balanced accuracy. AUPRC evaluates the model's ability to retrieve positive samples across different thresholds and is more suitable for detection tasks with rare positive events. In this experiment, the AUPRC of 0.7288 suggests that the model learned useful discriminative EEG patterns. The balanced accuracy of 0.8260 further indicates that the model performance is not solely driven by majority-class prediction.

## 5. Strengths of the Project

This project has several strengths.

First, the experimental workflow is reproducible. Environment setup, data preparation, GPU checking, model training, and submission-file generation are organized into a clear Google Colab pipeline, making the project easier to rerun and modify.

Second, the evaluation metrics are aligned with the characteristics of the task. Since IED detection is class-imbalanced, the project does not rely only on accuracy. Instead, it uses AUPRC as the main evaluation metric and also reports Binary-F1, Macro-F1, Weighted-F1, and Balanced Accuracy.

Third, the project includes a complete submission-generation pipeline. After training, the code can automatically load the best checkpoint, predict candidate EEG data, and generate a `sample_submission.zip` file for CodaBench submission.

Fourth, the project includes data-integrity checks. The code verifies the number of EEG `.npy` files, sample shape, number of candidate samples, and GPU status, reducing the risk of runtime issues caused by incorrect data paths or missing files.

## 6. Current Limitations

Although this project completes a full EEG-only IED detection workflow, it still has several limitations.

First, the current experiment uses the official baseline model `legacy/tcnet_eeg`. This is a relatively lightweight temporal convolutional network and serves as an initial baseline, but there is still room for improvement in extracting complex spatial-temporal EEG features.

Second, the experiment currently uses only one random seed. Multi-seed experiments have not yet been conducted, so the stability of the model performance has not been fully evaluated.

Third, the project has not systematically compared different EEG backbones. Stronger models such as DenseNet-121 and ResNet-50 have not yet been fully evaluated, and model ensembling has not yet been attempted.

Fourth, explicit class-imbalance optimization strategies have not yet been included in the training process. For example, class-weighted loss, focal loss, oversampling, or other minority-class optimization methods have not yet been applied.

Fifth, model interpretability analysis is still limited. The current project mainly focuses on model training and validation metrics and has not yet analyzed the contribution of different EEG channels, time windows, or waveform features to model predictions.

## 7. Future Improvements

The project can be further improved in several directions.

First, stronger EEG backbones such as DenseNet-121 and ResNet-50 can be explored. DenseNet-121 may strengthen multi-level feature reuse through dense connections and help capture fine-grained EEG waveform features. ResNet-50 can support the training of deeper models through residual connections and may improve the extraction of local waveform and cross-channel features.

Second, class-imbalance handling methods such as class-weighted loss, focal loss, or oversampling can be introduced to improve the recognition of IED-positive samples.

Third, EEG data augmentation methods can be explored, such as Gaussian noise, time shifting, channel dropout, or mixup, to improve model generalization.

Fourth, multi-seed experiments can be conducted, and the mean and standard deviation can be reported to provide a more stable evaluation of model performance.

Fifth, model ensembling can be attempted by combining predictions from multiple backbones or multiple random seeds to improve final prediction performance.

Sixth, interpretability analysis can be added, such as channel-importance analysis, temporal occlusion experiments, saliency maps, or other explainability methods, to better understand the discriminative EEG patterns learned by the model.

Seventh, the project can be extended to other NeuroMM-2026 tasks, such as multimodal detection tasks that incorporate video features, further developing the EEG-only pipeline into a multimodal neural-signal analysis workflow.
