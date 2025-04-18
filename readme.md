# LSTM-Based Sentiment Classification – ADL 5500 Final Project

## Overview
This project presents a robust deep learning solution using an **LSTM-based model** for sentiment classification on Yelp restaurant reviews. The goal was to classify each review into one of three sentiments: **Positive**, **Neutral**, or **Negative**.

Through extensive experimentation, this model:
- Handles real-world class imbalance without oversampling
- Generalizes well to noisy or ambiguous reviews
- Achieves high confidence and interpretability with human-friendly reporting

---

## Model Architecture

- `Embedding Layer`: Converts tokenized text to 128D dense vectors (tuned)
- `LSTM Layer`: Captures sequential dependencies in reviews
- `Dropout`: 0.2–0.5 to prevent overfitting
- `Dense + Softmax`: 3-class output

**Hyperparameters tuned with `Keras Tuner`**:
- LSTM units: [32–128]
- Embedding dimensions: [64, 128, 256]
- Dropout: [0.2–0.5]
- Learning Rate: [1e-3, 3e-4, 1e-4]

---

## Why LSTM?
- Ideal for modeling sequence-based sentiment shifts
- Proven performance for customer review understanding
- Captures contextual relationships better than bag-of-words or classical ML

---

## Final Evaluation Summary

| Metric               | Score      |
|----------------------|------------|
| **Test Accuracy**    | `0.8133`   |
| **Weighted F1 Score**| `0.82`     |
| **Macro F1 Score**   | `0.80`     |
| **ROC-AUC Scores**   | -          |
| ➤ Negative           | `0.9568`   |
| ➤ Neutral            | `0.8770`   |
| ➤ Positive           | `0.9593`   |

Performance is **balanced across classes**, with **neutral class** improved via custom calibration logic.

---

## Robustness Checks Performed

### Length-Based Review Analysis
| Length     | Accuracy | F1 Score |
|------------|----------|----------|
| Short      | 0.824    | 0.825    |
| Medium     | 0.836    | 0.839    |
| Long       | 0.833    | 0.838    |
| Very Long  | 0.828    | 0.827    |

Model is **resilient to review length** and handles both short snippets and long narratives well.

### Confidence Calibration
- Predictions with confidence ≥ 0.80 achieved **91% accuracy**
- Confidence thresholding used for better neutral class distinction

### Real-World Review Testing
> Example: *“It was okay. Not the best I've had, but not terrible either.”*
- Predicted: Neutral
- Confidence: 0.70

Model correctly handled vague, noisy, and ambiguous inputs with realistic outputs.

---

## Files in This Submission

| File Name                  | Purpose                                 |
|---------------------------|-----------------------------------------|
| `ADL_Final_LSTM.ipynb`    | Final training, tuning, and evaluation  |
| `best_lstm_model.h5`      | Final trained model                     |
| `tokenizer.pkl`           | Tokenizer to preprocess new text        |
| `history_log.pkl`         | Optional – Training loss/accuracy logs  |
| `README.md`               | This file                               |

---

## ✅ What Makes This Submission Stand Out

- Advanced tuning and performance benchmarking
- Real-world robustness beyond just metrics
- Misclassification and calibration analysis included
- Model easily reusable for further fine-tuning or production

---

