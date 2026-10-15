# DistilBERT Sentiment Classification — Experiment Guide

Part of the team [Yelp sentiment-classification project](README.md).

## Implementation
Open [ADL_DistilBert_Final_Project.ipynb](ADL_DistilBert_Final_Project.ipynb). The notebook fine-tunes `distilbert-base-uncased` with Hugging Face Transformers and PyTorch for three sentiment classes. It includes weighted cross-entropy, training-configuration search and error-analysis cells.

This is fine-tuning a pretrained transformer, not pretraining a foundation model.

## Recorded held-out test output

| Metric | Saved result |
|---|---:|
| Accuracy | 0.8128 |
| Weighted F1 | 0.8129 |
| Macro F1 | 0.7955 |
| Negative F1 | 0.8575 |
| Neutral F1 | 0.6606 |
| Positive F1 | 0.8685 |
| Test support | 7,752 |

These figures come from the committed classification-report output. They replace the earlier unsupported 88.1% weighted-F1 summary. The documentation update did not retrain the model.

## Inputs and reproducibility
The notebook loads `DistillBert_datasets/train.csv`, `val.csv` and `test.csv`. Those generated CSVs and model weights are not in the repository. Review the preprocessing notebook, data-access requirements, package imports, local paths and checkpoint selection before rerunning.

## Analysis and limits
The notebook explores confident mistakes, review-length effects and model explanations. In the saved test report, neutral sentiment is more difficult than positive or negative sentiment. High-confidence mistakes illustrate that softmax confidence is not a correctness guarantee.

Length-bucket analysis uses a broader cleaned dataset and should not be presented as a held-out benchmark. The [main README](README.md) describes pre-split augmentation concerns, actual file contents and next steps for a controlled LSTM/DistilBERT comparison.
