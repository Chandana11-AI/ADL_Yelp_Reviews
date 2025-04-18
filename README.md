# 💬 Yelp Sentiment Classification using LSTM and DistilBERT

> 🔍 **Course:** AIGC5005 - Advanced Deep Learning  
> 📝 **Project Type:** Final Project  
> 👩‍💻 **Team Size:** 3 Members   (Chandana, Devi, Jaspreet)
> 📂 **Domain:** Natural Language Processing  
> 📈 **Task:** Multiclass Sentiment Classification (Positive, Negative, Neutral)

---

## 📌 Project Overview

This project presents a comparative analysis of two deep learning models—**LSTM** and **DistilBERT**—to classify sentiment in Yelp reviews. The task is to predict whether a review expresses **positive**, **negative**, or **neutral** sentiment. We explore the performance of both models on reviews of varying lengths and vocabularies specific to the hospitality domain.

---

## 🎯 Objectives

- Build a cleaned and balanced Yelp review dataset.
- Train an LSTM model from scratch using embeddings.
- Fine-tune a pre-trained DistilBERT model.
- Evaluate both models on standard metrics (Accuracy, Precision, Recall, F1).
- Compare interpretability using SHAP and LIME visualizations.
- Tune hyperparameters to optimize each model.

---

## 🗂 Repository Structure

ADL_Final_Project/

├── data/                            # Preprocessed Yelp dataset
├── notebooks/                       # Jupyter Notebooks
│   ├── 1_LSTM_Model.ipynb           # LSTM model implementation
│   ├── 2_DistilBERT_Model.ipynb     # DistilBERT fine-tuning
├── outputs/                         # Model outputs and logs
│   ├── All plots and images
│   └── training_logs/               # Loss/accuracy tracking logs
├── saved_models/                    # Best model checkpoints
│   ├── best_model_lstm/             # Saved LSTM model
│   └── best_model_distilbert/       # Saved DistilBERT model
├── report/                          # Final PDF report
│   └── AIGC5005_Final_Report.pdf
├── requirements.txt                 # Project dependencies
└── README.md                        # This README file




