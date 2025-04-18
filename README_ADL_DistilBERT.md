# Sentiment Analysis on Yelp Reviews using DistilBERT

## Overview  
This project demonstrates how to classify Yelp reviews into **Positive**, **Neutral**, or **Negative** sentiments using a fine-tuned **DistilBERT** transformer model.  
It also includes interpretability via **LIME** and **SHAP**, performance diagnostics by **review length**, and detailed misclassification analysis.

---

## 🛠Tools & Tech  
- Python  
- Hugging Face Transformers  
- PyTorch  
- Scikit-learn  
- Matplotlib, SHAP, LIME  
- Google Colab (GPU: A100)

---

## Key Features  
- Cleaned and augmented Yelp review dataset with class imbalance handling  
- Fine-tuned DistilBERT on 3 sentiment classes  
- Hyperparameter tuning using grid search across 7 parameters  
- Evaluation across buckets of review lengths (short, medium, long, very long)  
- Misclassification audit with confidence-based filtering  
- Model interpretability via SHAP and LIME  
- Training and evaluation metric visualization (loss, F1, calibration)

---

## Results  
- Best Model: DistilBERT  
- Weighted F1 Score: **88.1%**  
- Best accuracy on **long reviews**: 85.7%  
- Slight performance drop on **very long reviews**: 83.6%  
-  LIME/SHAP confirmed that the model focuses on meaningful sentiment terms

---

##  How to Run  
```bash
pip install transformers torch shap lime scikit-learn matplotlib
```

Run the notebook:  
`ADL_DistilBert_Final_Project.ipynb` in Google Colab (A100 GPU recommended)

---

## Files  
- `ADL_DistilBert_Final_Project.ipynb`: Main notebook  
- `cleaned_yelp_reviews.csv`: Preprocessed labeled dataset  
- `model.safetensors`, `config.json`: Best model and configuration  
- `log_history.json`, `metrics.csv`: Training logs and evaluation scores  
- `.png` files: Plots for training curves, review length accuracy, calibration, SHAP explanations  
- `distilbert_project_bundle.zip`: Final zipped folder with all important outputs

---

## 📌 Status  
✅ Completed and ready for presentation/demo
