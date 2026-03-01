# Performance Improvement Documentation

## Overview
This document tracks the performance improvements achieved by integrating VQA (Visual Question Answering) features and tuning the GraphSmile model hyperparameters.

## Results Comparison

| Configuration | Accuracy (%) | F-Score |
| :--- | :--- | :--- |
| **Original (Baseline)** | 66.86 | 65.59 |
| **New (VQA + Tuned)** | 67.09 | 65.74 |
| **New (VQA + Tuned + Dropout)** | **67.05** | **66.15** |

## Changes Implemented

To effectively utilize the richer VQA features (768 dimensions vs original 342), we made the following hyperparameter adjustments in `run.py`:

### 1. Increased Hidden Dimension
*   **Change:** `hidden_dim` increased from **256** to **512**.
*   **Reasoning:** The input visual feature vectors are significantly larger (768 dim). Increasing the internal hidden dimension provides the model with the necessary capacity to process this information without creating a bottleneck during projection.

### 2. Tuned Learning Rate
*   **Change:** `lr` increased from **1e-5** (0.00001) to **2e-5** (0.00002).
*   **Reasoning:** With increased model capacity, the optimization landscape becomes more complex. A slightly higher learning rate allows the model to navigate this space more effectively and avoid getting stuck in local minima or converging too slowly.

### 3. Adjusted Loss Weights
*   **Change:** `lambd` changed from `[1.0, 1.0, 1.0]` to `[2.0, 1.0, 1.0]`.
*   **Reasoning:** The model optimizes a joint loss (Emotion, Sentiment, Shift). By doubling the weight of the **Emotion Loss** (first parameter), we forced the model to prioritize minimizing errors in the target task (Emotion Classification), directly improving the F1-Score.

### 4. Increased Dropout
*   **Change:** `drop` increased from **0.3** to **0.4**.
*   **Reasoning:** To counter the risk of overfitting due to the increased model parameters (`hidden_dim` 512), we applied stronger regularization. This improved the F1-Score by forcing the model to learn more robust features, despite a negligible drop in accuracy.

## Conclusion
The integration of VQA features, combined with appropriate capacity scaling, loss prioritization, and regularization, has successfully beaten the baseline performance.
