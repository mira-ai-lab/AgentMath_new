# 📐 多模态多智能体数学推理基准 [![Data Scale](https://img.shields.io/badge/Samples-1003-orange.svg)](#dataset-statistics) [![Modalities](https://img.shields.io/badge/Modalities-4-purple.svg)](#key-features) 
---
**面向大模型多智能体系统的多模态数学推理专用基准** 致力于解决传统多智能体数学问答基准**单模态限制、仅结果导向评估、缺乏协作鲁棒性测试**三大核心缺陷，为多智能体数学推理提供全维度、细粒度、过程化的评估能力。
## 🎯 基准定位 
MAMath-Bench 聚焦**LLM 多智能体协作数学推理**场景，覆盖文本/图像/音频/图文4类输入形式，通过**阶梯难度+过程里程碑+对抗负样本**，全面测评智能体团队的： 
- 多模态信息融合与对齐能力 
- 多智能体分步推理与协作验证能力 
- 错误检测、诊断与自恢复能力 
<div align="center"> <!-- 点击图片会跳转到PDF，target="_blank"会在新标签页打开 --> <a href="images/fig11.pdf" target="_blank"> <img src="images/fig11.png" width="50%" alt="难度-模态-步骤复杂度分布"> </a> <p>图1：难度、模态与推理步骤复杂度分布</p> </div>


## ✨ 核心特性 
1. **🌐 首创多模态输入设计** 
突破纯文本限制，支持文本、图像、音频、图文4种输入组合，还原真实数学问题场景。
2. **📝 细粒度推理里程碑标注** 为800个正样本提供**分步逻辑里程碑**，支持推理过程质量评估，而非仅看最终答案。
3. **⚠️ 对抗性负样本构建** 203个高精度负样本，包含**5类逻辑缺陷**，精准测试智能体协作中的错误抗性与恢复力。 
4. **📊 三级难度梯度** 按推理链长度与模态复杂度分级：简易(1–3步)、中等(4–6步)、困难(6–20步)，覆盖从小学到IMO竞赛难度。 
5. **🧮 全领域数学覆盖** 覆盖几何、代数、组合数学、数论等**11大数学领域**，全面检验推理上限。
--- 
## 📊 数据集统计
| 统计项 | 数值 |
| :--- | :--- | 
| 总样本数 | 1003 | 
| 正样本 | 800 | 
| 负样本 | 203 | 
| 简易难度 | 200 | 
| 中等难度 | 300 | 
| 困难难度 | 300 | 
| 模态类型 | 5种 | 
| 数学领域 | 11类 | 
| 负样本缺陷类型 | 5种 | 
### 负样本缺陷分布 
| 缺陷类型 | 样本数 | 占比 | 
| :--- | :--- | :--- | 
| 错误步骤(Wrong Steps) | 83 | 40.9% | 
| 冗余步骤(Redundant Steps) | 30 | 14.8% | 
| 重复步骤(Repeated Steps) | 30 | 14.8% | 
| 缺失步骤(Missing Steps) | 30 | 14.8% | 
| 顺序错乱(Swapped Steps) | 30 | 14.8% | 
### 数学领域分布
<div align="center">
  <img src="images/fig5.svg" width="50%" alt="数学领域分布">
  <p>图2：数学领域分布</p>
</div>

## 📂 数据结构
``` MAMath-Bench/ 
├── data/ # 核心数据目录 
│   ├── positive.json # 800个正样本 
│   ├── negative.json # 203个对抗负样本 
│   ├── img/ # 图片、图表等资源 
│   |  ├── 9.png
│   |  ├── 11.jpg
│   |  └── ...
|   ├── audio/ # 音频
|   |  ├── 16.mp3
|   |  ├── 17.mp3
|   |  └── ...
├
└── README.md
``` 

## 📑 数据格式

| 字段 | 类型 | 说明 | 示例 |
|---|---|---|---|
| `id` | Integer | 每个样本的唯一标识符。 | `1` |
| `task_domain` | String | 任务所属的数学领域，例如：代数（algebra）、几何（geometry）、证明（proof）、逻辑（logic）等。 | `"algebra"` |
| `input_modality` | String | 输入模态类型，例如：`text_only`、`text_image`、`image_audio`、`video_only` 等。 | `"image_audio"` |
| `problem_text` | String | 题目的文本描述，支持使用 LaTeX 表示数学公式。 | `"Compute $\sum_{n=1}^\infty \dots$"` |
| `problem_image` | List[String] | 题目关联的图像文件列表；如果没有图像输入，则为空列表 `[]`。 | `["11.jpg"]` |
| `audio_file` | String / NaN | 题目关联的音频文件（`.mp3`）；如果不存在则为 `NaN` 或 `null`。 | `"16.mp3"` |
| `video_file` | String / NaN | 题目关联的视频文件（`.mp4`）；如果不存在则为 `NaN` 或 `null`。 | `"6.mp4"` |
| `final_answer` | Mixed | 问题的最终答案，可以是数字、字符串或 LaTeX 表达式。 | `"2"`、`"19"`、`"$\frac{\pi}{12}$"` |
| `reasoning_steps` | String / NaN | 详细的解题推理过程（Step-by-step reasoning）。 | `"Because $y = \dots$"` |
