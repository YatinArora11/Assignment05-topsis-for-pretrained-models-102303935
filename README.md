# Topsis-for-Pretrained-Summarization-Models
**Submitted by:** Yatin Arora [102303935]

## Overview
This project applies the **TOPSIS** (Technique for Order of Preference by Similarity to Ideal Solution) decision-making method to rank pre-trained text summarization models. The goal is to find the "ideal" model that balances summary quality and linguistic accuracy with operational efficiency, such as low latency and storage requirements.

## Methodology
The TOPSIS analysis follows these steps:

1.  **Metric Selection**: We define a set of criteria to evaluate each model:
    * **ROUGE Score**: Measure of overlapping n-grams between generated and reference summaries (Benefit +).
    * **Inference Time (s)**: Time taken to generate a summary (Cost -).
    * **Model Size (MB)**: Storage requirement and memory footprint (Cost -).
    * **Summary Quality**: Simulated human-like quality metric for coherence (Benefit +).

2.  **Dataset Construction**: A decision matrix is created where rows are summarization models and columns are performance metrics.

3.  **Vector Normalization**: Each metric is normalized to a common scale to allow for valid comparison:
    $$r_{ij} = \frac{x_{ij}}{\sqrt{\sum_{k=1}^{m} x_{kj}^2}}$$

4.  **Weighted Normalization**: We apply importance weights to each metric based on standard deployment priorities:
    * ROUGE Score: 30%
    * Inference Time: 20%
    * Summary Quality: 25%
    * Compression Ratio: 15%
    * Model Size: 10%

5.  **Determine Ideal Solutions**:
    * **Ideal Best ($A^+$)**: Maximum value for benefits (ROUGE, Quality), Minimum for costs (Time, Size).
    * **Ideal Worst ($A^-$)**: Minimum value for benefits, Maximum for costs.

6.  **Calculate Separation Measures**: Euclidean distance of each model from $A^+$ ($S^+$) and $A^-$ ($S^-$).

7.  **Calculate TOPSIS Score**:
    $$P_i = \frac{S_i^-}{S_i^+ + S_i^-}$$
    A score closer to 1 implies the model is closer to the ideal solution for production use.

8.  **Ranking**: Models are ranked based on their TOPSIS score in descending order.

## Results Table
The final rankings based on the text summarization analysis are:

| Model | ROUGE Score | Inference Time (s) | Model Size (MB) | Quality | Topsis Score | Rank |
| :--- | :--- | :--- | :--- | :--- | :--- | :--- |
| **T5-Small** | 38.73 | 1.82 | 242 | 79.3 | **0.8167917956279168** | **1** |
| **T5-Base** | 42.05 | 2.91 | 892 | 84.7 | **0.6331160854031128** | **2** |
| **DistilBART** | 40.87 | 2.34 | 1200 | 82.1 | **0.5645729841641532** | **3** |
| **BART-Large** | 44.16 | 3.45 | 1625 | 88.5 | **0.3389681648604524** | **4** |
| **Pegasus** | 47.21 | 4.15 | 2280 | 91.2 | **0.18320820437208316** | **5** |

### Result Analysis
* **T5-Small** secures the **1st Rank**. While it has the lowest ROUGE score, its superior speed (1.82s) and minimal storage footprint (242MB) make it the most efficient choice for real-time applications.
* **Pegasus** and **BART-Large** provide the highest quality summaries and ROUGE scores; however, they are ranked lower (**4th and 5th**) due to their significant computational costs and slower inference times.
* **T5-Base** offers the most balanced "middle-ground" performance, maintaining competitive quality while remaining significantly faster than the heavier transformer architectures.

## Visualization
The following chart visually represents the TOPSIS scores across the different summarization architectures.

![Topsis Ranking](topsis_rank_plot.png)
