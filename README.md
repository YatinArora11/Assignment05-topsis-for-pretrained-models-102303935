# Topsis-for-Pretrained-Models
Submitted by: Yatin Arora [102303935]

## Overview
This project applies the **TOPSIS** (Technique for Order of Preference by Similarity to Ideal Solution) decision-making method to rank pre-trained text classification models. The goal is to find the "ideal" model that balances high accuracy with efficiency (low latency and model size).

## Methodology
The TOPSIS analysis follows these steps:

1.  **Metric Selection**: We define a set of criteria to evaluate each model.
    -   **Accuracy**: Measure of model performance (Benefit +)
    -   **Inference Time**: Time taken to predict (Cost -)
    -   **Model Size**: Storage requirement (Cost -)
    -   **F1 Score**: Harmonic mean of precision and recall (Benefit +)

2.  **Dataset Construction**: A decision matrix is created where rows are models and columns are metrics.

3.  **Vector Normalization**: Each metric is normalized to a common scale to allow for valid comparison.
    \[
    r_{ij} = \frac{x_{ij}}{\sqrt{\sum_{k=1}^{m} x_{kj}^2}}
    \]

4.  **Weighted Normalization**: We apply importance weights to each metric.
    -   Accuracy: 30%
    -   Inference Time: 20%
    -   Model Size: 20%
    -   F1 Score: 30%

5.  **Determine Ideal Solutions**:
    -   **Ideal Best ($A^+$)**: Maximum value for benefits, Minimum for costs.
    -   **Ideal Worst ($A^-$)**: Minimum value for benefits, Maximum for costs.

6.  **Calculate Separation Measures**: Euclidean distance of each model from $A^+$ ($S^+$) and $A^-$ ($S^-$).

7.  **Calculate TOPSIS Score**:
    \[
    P_i = \frac{S_i^-}{S_i^+ + S_i^-}
    \]
    A score closer to 1 implies the model is closer to the ideal solution.

8.  **Ranking**: Models are ranked based on their TOPSIS score in descending order.

## Results Table
The final rankings based on the analysis are:

| Model | Accuracy | Inference Time (ms) | Model Size (MB) | F1 Score | Topsis Score | Rank |
| :--- | :--- | :--- | :--- | :--- | :--- | :--- |
| **MobileBERT** | 0.88 | 50 | 100 | 0.87 | **0.9128** | **1** |
| **DistilBERT** | 0.89 | 90 | 260 | 0.88 | **0.6586** | **2** |
| **BERT-Base** | 0.92 | 150 | 440 | 0.91 | **0.2448** | **3** |
| **RoBERTa-Base** | 0.94 | 160 | 500 | 0.93 | **0.1571** | **4** |
| **XLNet** | 0.93 | 180 | 550 | 0.92 | **0.0737** | **5** |

### Result Analysis
-   **MobileBERT** secures the **1st Rank**. Despite having slightly lower accuracy than RoBERTa, its extremely low inference time (50ms) and small model size (100MB) make it the most "ideal" solution when efficiency is weighted significantly.
-   **RoBERTa-Base** and **XLNet**, while having high accuracy, are penalized heavily for their slow inference speeds and large sizes, leading to lower rankings (4th and 5th).

## Visualization
The following bar chart visually represents the TOPSIS scores.

![Topsis Ranking](topsis_rank_plot.png)

## Notebook
A Jupyter Notebook `Topsis_Analysis.ipynb` is included in this repository.
