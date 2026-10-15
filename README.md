# Yelp Review Sentiment Classification: LSTM and DistilBERT

A team project comparing a recurrent neural network trained for sentiment classification with a fine-tuned pretrained transformer on Yelp restaurant/hotel reviews.

**Context:** Humber advanced deep-learning course project.  
**Team:** Chandana, Devi and Jaspreet.  
**Task:** classify a review as negative, neutral or positive.

## Why this problem matters

Customer reviews contain complaints, praise and mixed opinions. A sentiment classifier can help summarize feedback, but an overall accuracy score can hide poor handling of neutral or ambiguous text. This project examines class-level errors, confidence and review-length effects alongside aggregate metrics.

## Data and label construction

The preprocessing workflow reads a 100,000-row review chunk, loads business records, joins on `business_id` and filters restaurant/hotel categories. It maps star ratings to three classes:

| Stars | Label | Meaning |
|---|---|---|
| 1–2 | 0 | Negative |
| 3 | 1 | Neutral |
| 4–5 | 2 | Positive |

Star ratings are proxy labels: a review can express mixed opinions even when its rating belongs to a single category.

The preprocessing artifacts include cleaning, class-balancing/augmentation experiments and model-specific tokenization. The code creates stratified 80/10/10 splits. The notebooks depend on external CSVs and Google Drive paths, which must be reconciled before rerunning.

## Model approaches

### LSTM trained for this task

A TensorFlow/Keras model uses learned embeddings, an LSTM, dropout and dense layers ending in a three-class softmax. Keras Tuner explores model settings, and training uses class weights, early stopping/checkpointing and validation monitoring.

This is task-specific neural-network training with library layers, not the creation of a new foundation model.

### Fine-tuned DistilBERT

The transformer experiment starts from `distilbert-base-uncased`. It uses Hugging Face Transformers and PyTorch, class-weighted cross-entropy and a hyperparameter search covering learning rate, dropout and other training settings.

This adapts an existing pretrained model; it does not pretrain DistilBERT from scratch.

## Recorded experiment results

The following values are read from saved outputs in the committed model notebooks. They have not been independently reproduced in this documentation update.

| Model notebook | Test accuracy | Weighted F1 | Test examples |
|---|---:|---:|---:|
| LSTM | 0.8305 | approximately 0.83, rounded in the classification report | 7,752 |
| DistilBERT | 0.8128 | 0.8129 | 7,752 |

The LSTM notebook reports a separate best tuning validation accuracy of approximately 0.8335. Validation and test figures represent different stages and should not be interchanged.

In these saved runs, the LSTM has higher aggregate test accuracy. That does not establish that LSTMs generally outperform transformers or that these runs form a fully controlled benchmark.

### What the class-level analysis shows

- DistilBERT's saved report gives neutral-class F1 of 0.6606, compared with 0.8575 for negative and 0.8685 for positive.
- The LSTM report rounds neutral F1 to 0.71, compared with 0.88 for negative and 0.87 for positive.
- Misclassification analysis includes confident wrong predictions, showing why confidence alone is not a reliability guarantee.
- Length-bucket analysis explores differences among short, medium, long and very long reviews. It is a separate diagnostic and should not be substituted for the held-out test result.

Earlier README values from other summaries differ from these saved outputs. This overview uses the notebook test outputs above.

## Repository guide

| File | Purpose |
|---|---|
| [ADL_Data_preprocessing.ipynb](ADL_Data_preprocessing.ipynb) | Data preparation, label construction and tokenization experiments |
| [ADL_LSTM_Final_Project.ipynb](ADL_LSTM_Final_Project.ipynb) | LSTM training, tuning, evaluation and analysis |
| [ADL_DistilBert_Final_Project.ipynb](ADL_DistilBert_Final_Project.ipynb) | DistilBERT fine-tuning, evaluation and error analysis |
| [data/Preprocessing](data/Preprocessing) | Additional notebook-style preprocessing export |
| [readme.md](readme.md) | LSTM experiment guide |
| [README_ADL_DistilBERT.md](README_ADL_DistilBERT.md) | DistilBERT experiment guide |

The public tree does not include the previously advertised `saved_models/`, `outputs/`, `report/` or `requirements.txt` paths. Model files and generated datasets referenced by notebooks are external artifacts, not committed files.

## Reproduce the workflow

1. Obtain the Yelp academic dataset in accordance with its terms.
2. Open the preprocessing notebook in Jupyter or Google Colab.
3. Review Drive paths, required language resources and the data-cleaning/augmentation choices.
4. Generate the model-specific splits and reconcile folder names: preprocessing writes `LSTM_dataset/`, while the LSTM notebook reads `lstm_data/`.
5. Run each model notebook with the same documented split and label mapping.
6. Record the exact package versions, random seeds, checkpoint choice and final untouched test evaluation.

The LSTM notebook uses TensorFlow/Keras and Keras Tuner. DistilBERT uses PyTorch, Transformers, datasets and scikit-learn. Additional cells use visualization and explanation libraries. Review imports before running; no pinned environment is provided.

## Evaluation caveats

- Augmentation/balancing occurs before splitting in the preprocessing experiment. Related original/augmented examples may therefore enter different splits; a stronger evaluation splits original reviews first and augments training data only.
- Tokenizer/vocabulary construction should also be restricted to training data where it is learned from this dataset.
- Review-length diagnostics load a broader cleaned CSV. Those diagnostics are not necessarily restricted to the test partition.
- Notebooks retain multiple experimental stages; rerun from a clean kernel and record which checkpoint generated each metric.
- SHAP/LIME are explored as explanations, but explanations do not validate a prediction or establish causal reasoning.

## My contribution and skills

As a member of the three-person team, I contributed to the sentiment-classification study and its model-development/evaluation workflow. The notebooks preserve the team's experiments; this page does not attribute every cell or all team results to me individually.

Skills represented include NLP preprocessing, supervised learning, LSTM training, transformer fine-tuning, class imbalance, hyperparameter tuning, model comparison and technical communication.

## Next steps

Create a train-only preprocessing pipeline, deduplicate before splitting, pin dependencies, publish a reproducibility manifest and compare both models against a simple TF-IDF baseline on one frozen test set.
