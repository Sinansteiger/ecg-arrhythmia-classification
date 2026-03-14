# 🫀 ECG Arrhythmia Classification

![Python](https://img.shields.io/badge/Python-3.10+-blue?style=flat-square&logo=python)
![Scikit-Learn](https://img.shields.io/badge/Scikit--Learn-1.3+-orange?style=flat-square&logo=scikit-learn)
![Pandas](https://img.shields.io/badge/Pandas-2.0+-150458?style=flat-square&logo=pandas)
![License](https://img.shields.io/badge/License-MIT-green?style=flat-square)
![Status](https://img.shields.io/badge/Status-Complete-brightgreen?style=flat-square)

> A machine learning pipeline for automated ECG arrhythmia classification using the MIT-BIH dataset. Implements statistical feature engineering and a Random Forest classifier to distinguish between 5 cardiac rhythm categories.

---

## 📌 Overview

This project implements an end-to-end **ECG signal classification pipeline** on the publicly available [MIT-BIH Arrhythmia Dataset](https://www.kaggle.com/datasets/shayanfazeli/heartbeat). Each sample represents a single heartbeat encoded as 187 time-series values. The goal is to automatically classify each heartbeat into one of five arrhythmia categories — a clinically relevant task in automated cardiac monitoring.

---

## 🎯 Key Results

| Metric | Value |
|---|---|
| Dataset | MIT-BIH Arrhythmia (87,554 samples) |
| Features | 10 statistical/morphological features per beat |
| Classifier | Random Forest (100 estimators, balanced weights) |
| Train/Test Split | 80% / 20% stratified |
| Overall Accuracy | **87%** |

---

## 🔬 Results & Interpretation

> **What do these numbers mean?** Here is a plain-language explanation of the model's performance — no machine learning background required.

### Overall Accuracy: 87%

The model correctly identified the heartbeat type **87 out of every 100 times**. This is a strong result for a statistical feature-based approach (without deep learning).

### Per-Class Performance

| Class | Precision | Recall | What it means in plain language |
|---|---|---|---|
| **Normal (N)** | 96% | 90% | The model is excellent at recognizing healthy heartbeats. Out of 14,494 normal beats in the test set, it correctly identified 90% of them. |
| **Ventricular (V)** | 74% | 68% | Ventricular ectopic beats (a common arrhythmia) were detected reasonably well. These beats look visually distinct, which helps. |
| **Unknown (Q)** | 71% | 85% | Unclassifiable beats were caught well — the model is cautious and flags unusual patterns. |
| **Supraventricular (S)** | 28% | 58% | Performance is lower here. This class has very few training examples (only ~500 out of 87,000), making it hard for the model to learn. |
| **Fusion (F)** | 24% | 73% | Similar challenge — Fusion beats are rare and visually resemble a mix of other classes. |

### Why are some classes harder to detect?

The dataset is highly **imbalanced** — 83% of all heartbeats are Normal, while Supraventricular and Fusion beats together make up less than 1%. This is actually realistic: in a real ECG recording, most beats are normal. However, it means the model has seen far fewer examples of rare arrhythmias, making them harder to learn.

> 💡 **Clinical relevance:** In a real-world screening tool, high recall on dangerous arrhythmias (Ventricular, Supraventricular) is more important than overall accuracy. Missing a dangerous beat is more costly than a false alarm. This is why `class_weight='balanced'` was used in training — to give rare classes more importance.

---

## 🗂️ Project Structure

```
ecg-arrhythmia-classification/
│
├── notebooks/
│   └── ECG_Analysis_Enhanced.ipynb   # Full pipeline (annotated)
│
├── outputs/
│   ├── 01_ecg_classes.png            # Sample heartbeat per class
│   ├── 02_mean_signals.png           # Mean ± STD signal per class
│   ├── 03_class_distribution.png     # Class imbalance visualization
│   ├── 04_feature_correlation.png    # Feature correlation heatmap
│   ├── 05_confusion_matrix.png       # Classification results
│   └── 06_feature_importance.png     # Random Forest feature ranking
│
└── README.md
```

---

## ⚙️ Pipeline

```
Raw ECG CSV
      │
      ▼
[1] Load & Inspect         pd.read_csv()
      │                    → 87,554 samples × 188 columns
      ▼
[2] Signal Visualization   Per-class heartbeat plots
      │                    → Mean ± STD shading
      ▼
[3] Feature Engineering    10 features per heartbeat
      │                    → mean, std, min, max, range,
      │                       median, skewness, kurtosis,
      │                       energy, peak position
      ▼
[4] Train/Test Split       80% train / 20% test (stratified)
      │
      ▼
[5] Random Forest          100 estimators, balanced weights
      │                    → Handles class imbalance
      ▼
[6] Evaluation             Confusion matrix + classification report
                           → Per-class precision, recall, F1
```

---

## 🏷️ Arrhythmia Classes

| Class | Label | Description |
|---|---|---|
| 0 | Normal (N) | Normal sinus rhythm |
| 1 | Supraventricular (S) | Supraventricular ectopic beat |
| 2 | Ventricular (V) | Ventricular ectopic beat |
| 3 | Fusion (F) | Fusion beat |
| 4 | Unknown (Q) | Unclassifiable beat |

---

## 📊 Visualizations

### ECG Signal per Arrhythmia Class
![ECG Classes](outputs/01_ecg_classes.png)

### Mean ± STD Signal per Class
![Mean Signals](outputs/02_mean_signals.png)

### Class Distribution
![Class Distribution](outputs/03_class_distribution.png)

### Feature Correlation Matrix
![Feature Correlation](outputs/04_feature_correlation.png)

### Confusion Matrix
![Confusion Matrix](outputs/05_confusion_matrix.png)

### Feature Importance
![Feature Importance](outputs/06_feature_importance.png)

---

## 🚀 Getting Started

### 1. Clone the repository
```bash
git clone https://github.com/Sinansteiger/ecg-arrhythmia-classification.git
cd ecg-arrhythmia-classification
```

### 2. Install dependencies
```bash
pip install pandas numpy matplotlib seaborn scikit-learn jupyter
```

### 3. Download the dataset
Download `mitbih_train.csv` from [Kaggle — Heartbeat Dataset](https://www.kaggle.com/datasets/shayanfazeli/heartbeat) and place it in the project root.

### 4. Run the notebook
```bash
jupyter notebook notebooks/ECG_Analysis_Enhanced.ipynb
```

> **Note:** Update the `pd.read_csv()` path in the notebook to match your local directory.

---

## 🧰 Dependencies

| Package | Purpose |
|---|---|
| `pandas` | Data loading and manipulation |
| `numpy` | Numerical computation |
| `matplotlib` | Signal and result visualization |
| `seaborn` | Statistical plots |
| `scikit-learn` | Random Forest, metrics, train/test split |
| `jupyter` | Interactive notebook environment |

---

## 📚 References

1. Moody, G.B., Mark, R.G. (2001). The impact of the MIT-BIH Arrhythmia Database. *IEEE Engineering in Medicine and Biology*, 20(3), 45–50.
2. Goldberger, A., et al. (2000). PhysioBank, PhysioToolkit, and PhysioNet. *Circulation*, 101(23).
3. Fazeli, S. (2018). Heartbeat Dataset. *Kaggle*. https://www.kaggle.com/datasets/shayanfazeli/heartbeat
4. Breiman, L. (2001). Random Forests. *Machine Learning*, 45, 5–32.

---

## 📄 License

This project is licensed under the MIT License.

---

## 🙋 Author

**Sinan Berke AKÇA**
- GitHub: [@Sinansteiger](https://github.com/Sinansteiger)
- LinkedIn: [linkedin.com/in/sinan-berke-akça-623716256](https://www.linkedin.com/in/sinan-berke-akça-623716256)

---
