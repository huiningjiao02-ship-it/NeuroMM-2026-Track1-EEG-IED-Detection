# EXPERIMENT_DISCUSSION.md 中文版

## 1. 任务概述

本项目针对 NeuroMM-2026 Challenge Track 1 中的 EEG-only IED detection 任务展开。该任务是一个基于脑电信号的二分类问题，目标是判断给定 EEG 片段中是否包含发作间期癫痫样放电，即 interictal epileptiform discharge，简称 IED 或 spike。

每个 EEG 样本为 29 通道、4 秒长度的脑电片段。模型需要根据 EEG 时序信号判断该片段属于 IED 阳性样本还是非 IED 样本。由于临床 EEG 数据中阳性 IED 事件通常远少于阴性样本，因此该任务存在明显的类别不平衡问题。

## 2. 实验设置

本项目基于 NeuroMM-2026 官方 EEG-only baseline 代码进行实验，使用 Track 1 所对应的 EEG 数据和标注文件完成模型训练、验证和候选测试集预测。

实验设置如下：

| 项目 | 内容 |
|---|---|
| 任务 | Track 1 EEG-only IED detection |
| 模型 | legacy/tcnet_eeg |
| 训练轮数 | 30 epochs |
| 随机种子 | 0 |
| 学习率 | 5e-4 |
| 批大小 | 64 |
| 运行环境 | Google Colab GPU |

整体实验流程包括：安装官方 baseline 环境、解压 EEG 数据文件、加载训练/验证标注、整理 EEG 文件路径、训练 EEG-only 模型、在验证集上评估模型表现，并最终对 candidate EEG 数据生成预测结果，用于 CodaBench 提交。

## 3. 验证结果

在当前 baseline 实验设置下，模型在验证集上的结果如下：

| 指标 | 数值 |
|---|---:|
| AUPRC | 0.7288 |
| Binary-F1 | 0.6615 |
| Macro-F1 | 0.8118 |
| Weighted-F1 | 0.9332 |
| Accuracy | 0.9317 |
| Balanced Accuracy | 0.8260 |

其中，AUPRC 被作为主要评价指标，因为该任务存在类别不平衡问题。相比普通 Accuracy，AUPRC 更能反映模型对少数类 IED 阳性样本的识别能力。

## 4. 结果解释

虽然模型在验证集上的 Accuracy 达到 0.9317，但在类别不平衡任务中，高 Accuracy 需要谨慎解释。因为如果阴性样本占多数，模型即使倾向于预测多数类，也可能获得较高的准确率，但这并不一定说明模型真正有效识别了 IED 阳性样本。

因此，本项目更关注 AUPRC 和 Balanced Accuracy。AUPRC 可以评估模型在不同阈值下检索阳性样本的能力，适合用于阳性样本较少的检测任务。当前实验中，AUPRC 达到 0.7288，说明模型已经学习到一定具有判别性的 EEG 模式；Balanced Accuracy 达到 0.8260，说明模型表现并不完全依赖多数类预测。

## 5. 项目优点

本项目的主要优点包括以下几个方面：

第一，实验流程具有较好的可复现性。项目将环境安装、数据准备、GPU 检查、模型训练和提交文件生成整理为清晰的 Colab 流水线，便于重复运行和后续修改。

第二，评价指标与任务特点匹配。由于 IED 检测任务具有类别不平衡特点，项目没有只关注 Accuracy，而是将 AUPRC 作为主要评价指标，并同时报告 Binary-F1、Macro-F1、Weighted-F1 和 Balanced Accuracy。

第三，项目包含完整的提交文件生成流程。训练完成后，代码可以自动加载最佳 checkpoint，对 candidate EEG 数据进行预测，并生成可上传至 CodaBench 的 `sample_submission.zip` 文件。

第四，项目加入了数据完整性检查。代码会检查 EEG `.npy` 文件数量、样本形状、候选样本数量以及 GPU 状态，降低数据路径错误或文件缺失带来的运行问题。

## 6. 当前局限性

尽管本项目完成了完整的 EEG-only IED 检测流程，但目前仍存在一些局限性。

第一，目前使用的是官方 baseline 中的 `legacy/tcnet_eeg` 模型。该模型属于较轻量的时间卷积网络，能够作为初始 baseline，但在复杂 EEG 时空特征提取方面仍有提升空间。

第二，目前实验只使用单一随机种子，尚未进行多随机种子重复实验。因此，当前结果还不能充分反映模型性能的稳定性。

第三，项目尚未系统比较不同 EEG backbone 的性能。例如，DenseNet-121、ResNet-50 等更强模型尚未完成对比实验，也还没有尝试模型集成。

第四，当前训练过程尚未显式加入类别不平衡优化策略。例如，尚未使用 class-weighted loss、focal loss、过采样或其他针对少数类样本的优化方法。

第五，模型解释性分析仍然有限。当前项目主要关注模型训练和验证指标，尚未进一步分析不同 EEG 通道、时间窗口或波形特征对模型预测的贡献。

## 7. 后续改进方向

后续可以从以下几个方向继续优化本项目。

第一，可以尝试更强的 EEG backbone，例如 DenseNet-121 和 ResNet-50。DenseNet-121 可以通过密集连接加强多层特征复用，有助于捕捉细粒度 EEG 波形特征；ResNet-50 可以通过残差连接支持更深层网络训练，从而增强对局部波形和跨通道特征的提取能力。

第二，可以加入类别不平衡处理方法，例如 class-weighted loss、focal loss 或过采样策略，以提高模型对 IED 阳性样本的识别能力。

第三，可以尝试 EEG 数据增强方法，例如加入 Gaussian noise、time shifting、channel dropout 或 mixup，以提升模型泛化能力。

第四，可以进行多随机种子实验，并报告平均值和标准差，从而更稳定地评估模型性能。

第五，可以尝试模型集成，将多个 backbone 或多个随机种子的模型结果进行融合，以提高最终预测表现。

第六，可以加入模型解释性分析，例如通道重要性分析、时间窗口遮挡实验、saliency map 或其他可解释性方法，进一步理解模型在 EEG 信号中学习到的判别模式。

第七，后续可以扩展到 NeuroMM-2026 的其他任务，例如结合视频特征的多模态检测任务，从 EEG-only pipeline 进一步发展为多模态神经信号分析流程。
