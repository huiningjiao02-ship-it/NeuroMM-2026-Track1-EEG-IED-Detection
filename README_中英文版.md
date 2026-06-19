# NeuroMM-2026 Track 1: EEG-only IED Detection  
# NeuroMM-2026 Track 1：仅基于 EEG 的 IED 检测

## 1. Project Overview  
## 1. 项目概述

This repository contains a reproducible Google Colab pipeline for NeuroMM-2026 Challenge Track 1, also known as the NMM-Basic-IED task. The task is an EEG-only binary classification problem for detecting interictal epileptiform discharges (IEDs), or spike events, from EEG segments.

本仓库包含一个可复现的 Google Colab 流水线，用于 NeuroMM-2026 Challenge Track 1，也就是 NMM-Basic-IED 任务。该任务是一个仅基于 EEG 的二分类任务，用于从 EEG 片段中检测发作间期癫痫样放电，即 interictal epileptiform discharges，简称 IED 或 spike events。

The project builds a clean and reproducible deep-learning workflow for clinical EEG event detection, including environment setup, data preparation, model training, validation, and CodaBench submission-file generation.

本项目构建了一个整洁、可复现的深度学习工作流，用于临床 EEG 事件检测，流程包括环境配置、数据准备、模型训练、验证评估以及 CodaBench 提交文件生成。

## 2. Task Description  
## 2. 任务说明

The objective is to classify whether a given EEG segment contains an IED/spike event. Each EEG sample is a multi-channel EEG segment, and the model learns discriminative EEG patterns for distinguishing IED-positive samples from non-IED samples.

该任务的目标是判断给定 EEG 片段中是否包含 IED/spike 事件。每个样本为多通道 EEG 片段，模型需要学习具有判别性的 EEG 模式，以区分 IED 阳性样本和非 IED 样本。

Since IED detection is a class-imbalanced clinical EEG task, AUPRC is used as the primary evaluation metric, together with accuracy, balanced accuracy, and F1-based metrics.

由于 IED 检测属于类别不平衡的临床 EEG 任务，因此本项目将 AUPRC 作为主要评价指标，并同时报告 Accuracy、Balanced Accuracy 和 F1 相关指标。

## 3. Pipeline Structure  
## 3. 流水线结构

The project is organized into the following steps:

本项目整理为以下几个步骤：

1. Environment setup and official baseline installation;  
   环境配置与官方 baseline 安装；

2. Data preparation and EEG archive extraction;  
   数据准备与 EEG 压缩文件解压；

3. GPU and data-integrity checking;  
   GPU 与数据完整性检查；

4. EEG-only model training for Track 1;  
   Track 1 EEG-only 模型训练；

5. CodaBench submission-file generation.  
   CodaBench 提交文件生成。

## 4. Experimental Setting  
## 4. 实验设置

| Item | Setting |
|---|---|
| Task | Track 1 EEG-only IED detection |
| Model | legacy/tcnet_eeg |
| Epochs | 30 |
| Random seed | 0 |
| Learning rate | 5e-4 |
| Batch size | 64 |
| Runtime environment | Google Colab GPU |

| 项目 | 内容 |
|---|---|
| 任务 | Track 1 EEG-only IED detection |
| 模型 | legacy/tcnet_eeg |
| 训练轮数 | 30 epochs |
| 随机种子 | 0 |
| 学习率 | 5e-4 |
| 批大小 | 64 |
| 运行环境 | Google Colab GPU |

## 5. Validation Results  
## 5. 验证结果

| Metric | Value |
|---|---:|
| AUPRC | 0.7288 |
| Binary-F1 | 0.6615 |
| Macro-F1 | 0.8118 |
| Weighted-F1 | 0.9332 |
| Accuracy | 0.9317 |
| Balanced Accuracy | 0.8260 |

| 指标 | 数值 |
|---|---:|
| AUPRC | 0.7288 |
| Binary-F1 | 0.6615 |
| Macro-F1 | 0.8118 |
| Weighted-F1 | 0.9332 |
| Accuracy | 0.9317 |
| Balanced Accuracy | 0.8260 |

Because IED detection is class-imbalanced, AUPRC and balanced accuracy are more informative than raw accuracy. A high raw accuracy alone may be misleading if the model mainly predicts the majority non-IED class.

由于 IED 检测存在类别不平衡问题，AUPRC 和 Balanced Accuracy 比单纯 Accuracy 更有解释价值。如果模型主要预测多数类非 IED 样本，也可能得到较高 Accuracy，因此不能只依赖 Accuracy 评价模型。

## 6. Data Requirements  
## 6. 数据要求

The NeuroMM-2026 dataset is not included in this repository. Users need to request access from the official dataset source and follow the data-use agreement.

本仓库不包含 NeuroMM-2026 数据集。用户需要通过官方数据集渠道申请访问权限，并遵守相应的数据使用协议。

The following files are expected for running the pipeline:

运行该流水线需要准备以下文件：

| File | Description |
|---|---|
| `neuromm2026_train_val.csv` | Training and validation annotation file |
| `candidate_ids.txt` | Candidate test-sample IDs |
| `train_eeg.tar` | Training EEG data archive |
| `candidate_eeg.tar` | Candidate EEG data archive |

| 文件 | 含义 |
|---|---|
| `neuromm2026_train_val.csv` | 训练/验证标注文件 |
| `candidate_ids.txt` | 候选测试样本 ID |
| `train_eeg.tar` | 训练 EEG 数据压缩包 |
| `candidate_eeg.tar` | 候选测试 EEG 数据压缩包 |

The code supports the official `.tar` files and can also handle nested `.tar` files if the data have been accidentally repacked.

代码支持官方 `.tar` 文件，也可以处理由于意外重复打包而形成的嵌套 `.tar` 文件。

## 7. How to Run  
## 7. 运行方式

1. Open the notebook in Google Colab.  
   在 Google Colab 中打开 notebook。

2. Enable GPU runtime.  
   设置 GPU 运行环境。

3. Upload the required data files to Google Drive.  
   将所需数据文件上传至 Google Drive。

4. Make sure the filenames match the expected names.  
   确认文件名与项目要求一致。

5. Run the notebook cells in order.  
   按顺序运行 notebook 单元。

6. The final cell generates `sample_submission.zip` for CodaBench submission.  
   最后一个单元会生成 `sample_submission.zip`，用于上传至 CodaBench。

## 8. Notes for Research Use  
## 8. 研究使用说明

This repository is intended for academic and challenge-related experiments. It does not redistribute clinical EEG data, annotations, candidate data, checkpoints, submission files, or derived data.

本仓库仅用于学术研究和挑战赛相关实验，不重新分发临床 EEG 数据、标注文件、候选数据、模型权重、提交文件或衍生数据。

The model outputs should be used only for research and challenge evaluation. They should not be treated as medical diagnostic results.

模型输出仅适用于研究和挑战赛评估，不应被视为医学诊断结果。

## 9. Resume-Friendly Project Description  
## 9. 简历可用项目描述

Built a reproducible EEG-only IED detection pipeline using PyTorch, NumPy, and Pandas. Processed multi-channel EEG segments and implemented an official TCN baseline for binary interictal epileptiform discharge detection. Used AUPRC as the primary metric to address class imbalance, achieving a validation AUPRC of 0.7288 and balanced accuracy of 0.8260.

使用 PyTorch、NumPy 和 Pandas 构建可复现的 EEG-only IED 检测流水线，处理多通道 EEG 片段数据，并基于官方 TCN baseline 完成发作间期癫痫样放电二分类检测。针对类别不平衡问题，采用 AUPRC 作为主要评价指标，验证集 AUPRC 达到 0.7288，Balanced Accuracy 达到 0.8260。
