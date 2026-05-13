# 🔐 Network Intrusion Detection using Machine Learning
### CICIDS2017 Dataset — Binary & Multi-class Classification

---

## 📌 Project Overview

This project builds an **automated network intrusion detection system** using machine learning on the CICIDS2017 dataset. The goal is to classify network traffic as either benign or malicious, and identify the specific type of attack.

**Research Question:**
> *"Can we automatically detect malicious network traffic and classify the type of attack based on network flow features, while addressing the extreme class imbalance between attack and benign traffic?"*

---

## 📊 Dataset

- **Source:** [CICIDS2017 — Kaggle](https://www.kaggle.com/datasets/chethuhn/network-intrusion-dataset)
- **Size:** 2,830,743 records | 79 features
- **Attack Types:** 14 different attack types + BENIGN traffic
- **Challenge:** Severe class imbalance — 83% BENIGN vs 17% attacks

### Attack Types Include:
| Attack | Description |
|--------|-------------|
| DoS Hulk | HTTP flood attack |
| DDoS | Distributed Denial of Service |
| PortScan | Port scanning — reconnaissance |
| FTP/SSH-Patator | Brute force attacks |
| DoS Slowloris | Slow HTTP attack |
| Web Attack (XSS, SQLi, Brute Force) | Web application attacks |
| Heartbleed | OpenSSL vulnerability exploit |
| Bot | Botnet traffic |
| Infiltration | Internal network infiltration |

---

## 🔬 Methodology

### Data Pipeline:
1. **Data Loading** — 8 CSV files merged into one DataFrame
2. **EDA** — Exploratory Data Analysis & visualizations
3. **Data Cleaning** — Removed duplicates, NaN, infinite, negative values
4. **Feature Selection** — Removed 31 highly correlated features (>0.9)
5. **Label Encoding** — Binary (0/1) & Multi-class (0-14)
6. **Train/Val/Test Split** — 80/10/10 with stratification
7. **Standardization** — StandardScaler (fit on train only)
8. **Modeling** — 3 models compared
9. **Fine Tuning** — Randomized Search CV

---

## 🤖 Models

| Model | ROC-AUC | False Negatives | Training Time |
|-------|---------|-----------------|---------------|
| Logistic Regression | 0.9789 | 1,575 | 1.2 min |
| Random Forest | 0.9998 | 193 | 11 min |
| XGBoost | 0.9999 | 29 | 1 min |
| **XGBoost Tuned** | **1.0000** 🏆 | **8** 🏆 | **0.5 min** 🏆 |

---

## 🏆 Results

### Binary Classification (BENIGN vs ATTACK):
- **ROC-AUC: 1.0000** — Perfect score!
- **False Negatives: 8** out of 42,574 attacks — 0.02% miss rate
- **Total errors: 206** out of 252,070 records — less than 0.08%!

### Multi-class Classification (Attack Type Identification):
- **Macro F1: 0.86** | **Weighted F1: 1.00**
- Perfect F1 (1.00) on: DDoS, DoS Hulk, FTP-Patator, SSH-Patator
- Lower scores on rare attacks (SQL Injection: 3 records, Heartbleed: 11 records)

### Best Hyperparameters (after Fine Tuning):
```python
{
    'n_estimators': 200,
    'max_depth': 6,
    'learning_rate': 0.3,
    'subsample': 1.0,
    'colsample_bytree': 0.8
}
```

### Top 5 Most Important Features:
1. Destination Port (10.1%)
2. Bwd Packet Length Max (9.5%)
3. Init_Win_bytes_forward (7.7%)
4. Init_Win_bytes_backward (6.0%)
5. Total Length of Fwd Packets (5.4%)

---

## 🚀 How to Run

### Prerequisites:
1. Google Colab (recommended) or Jupyter Notebook
2. Kaggle account with API key

### Steps:
```bash
# 1. Clone the repository
git clone https://github.com/YOUR_USERNAME/network-intrusion-detection.git

# 2. Install dependencies
pip install -r requirements.txt

# 3. Set up Kaggle API
# Place kaggle.json in ~/.kaggle/

# 4. Open the notebook
jupyter notebook network_intrusion_detection.ipynb
```

### Running on Google Colab:
1. Upload `kaggle.json` when prompted
2. Run all cells sequentially
3. Dataset downloads automatically from Kaggle

---

## 📁 Repository Structure

```
network-intrusion-detection/
│
├── network_intrusion_detection.ipynb  # Main notebook
├── requirements.txt                    # Dependencies
└── README.md                          # This file
```

---

## 🛠️ Technologies Used

- **Python 3.10+**
- **Pandas** — Data manipulation
- **NumPy** — Numerical computing
- **Scikit-learn** — ML models & preprocessing
- **XGBoost** — Gradient boosting
- **Matplotlib & Seaborn** — Visualizations
- **Google Colab Pro** — Cloud computing (32GB RAM)

---

## 📈 Key Findings

1. **XGBoost dominates** — fastest and most accurate model
2. **Fine Tuning improved False Negatives by 72%** (29 → 8)
3. **Zero overfitting** — Validation AUC = Test AUC = 1.0000
4. **Rare attacks remain challenging** — SQL Injection (21 records) hard to detect
5. **Destination Port** is the most important feature for attack detection

---

## ⚠️ Limitations

- Dataset created in controlled lab environment (2017) — real-world performance may differ
- Rare attack classes (Heartbleed, SQL Injection) have very few samples
- Model trained on 2017 attack patterns — new attack vectors may not be detected

---

## 📚 References

- [CICIDS2017 Dataset Paper](https://www.unb.ca/cic/datasets/ids-2017.html)
- [XGBoost Documentation](https://xgboost.readthedocs.io/)
- [Scikit-learn Documentation](https://scikit-learn.org/)

---

*Built with ❤️ as part of a Data Science course project*
