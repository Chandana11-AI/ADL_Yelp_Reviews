# LSTM Sentiment Classification — Experiment Guide

This notebook is part of the team [Yelp sentiment-classification project](README.md).

## Implementation
Open [ADL_LSTM_Final_Project.ipynb](ADL_LSTM_Final_Project.ipynb). It uses TensorFlow/Keras embeddings, LSTM, dropout and dense layers for negative/neutral/positive classification. Keras Tuner, class weights and early stopping/checkpointing support the experiments.

## Recorded evaluation
The committed notebook prints **test accuracy 0.8305** and **test loss 0.4488** on 7,752 examples. Its classification report rounds weighted F1 to 0.83. Neutral sentiment is the weakest class, with rounded F1 of 0.71.

These are saved notebook outputs, not newly reproduced measurements. Older summary values should not be treated as the definitive final run.

## Running and interpretation
The notebook reads external split CSVs from `lstm_data/`. Reconcile that path with the preprocessing output `LSTM_dataset/`, install the notebook imports in an isolated environment and review checkpoint selection.

Training curves, class-level errors, confidence checks and review-length diagnostics answer different questions. Length analysis is not necessarily restricted to the held-out partition. See the [main README](README.md) for the pre-split augmentation/vocabulary caveats and a stronger evaluation protocol.

Model checkpoints, tokenizers and logs referenced by the notebook are not committed as standalone files. No production-deployment claim is made.
