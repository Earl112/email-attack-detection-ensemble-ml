# email-attack-detection-ensemble-ml
A machine learning-based system for detecting malicious and phishing emails using ensemble classification techniques, classifying email text as **phishing** or **safe** using an ensemble approach.

## Overview

Phishing emails are a common social-engineering threat. This project investigates whether text-based machine learning can identify phishing messages from their content.

The original implementation uses:

1. **TF-IDF** for text feature extraction
2. **StandardScaler** for feature scaling
3. **Principal Component Analysis (PCA)** to reduce dimensionality while retaining 90% variance
4. **Logistic Regression** as a base classifier
5. **Decision Tree** as a second base classifier
6. **Stacking Classifier** to combine the base models
7. **Precision, Recall and F1-score** for evaluation
8. **5-fold cross-validation** for an additional evaluation of the stacking model

## Dataset

The project uses the Kaggle **Phishing Email Detection** dataset by `yashrathod0909`

Dataset page:
https://www.kaggle.com/datasets/yashrathod0909/phishingemaildetection

The dataset contains email text and an email-type label. The notebook output used for the original project reports:

- **18,650 emails**
- **11,322 phishing emails**
- **7,328 safe/legitimate emails**
- **2 columns used:** `Email Text` and `Email Type`

The dataset is not committed to this repository by default. Download `Phishing_Email.csv` and place it at:

```text
data/Phishing_Email.csv
```

See [`data/README.md`](data/README.md).

> **Source discrepancy note:** The original project PDF states 18,600 samples, while the original notebook's recorded output shows 18,650. This repository uses the notebook output (18,650) because it is directly observed from the code run.

## Methodology

### 1. Data preprocessing

The dataset is reduced to the two relevant columns:

- `Email Text`
- `Email Type`

Missing email text values are replaced with empty strings, and the target labels are encoded using `LabelEncoder`.

### 2. TF-IDF

The email text is transformed into numerical features using:

```python
TfidfVectorizer(max_features=700)
```

This produces a maximum of **700 TF-IDF features**.

### 3. Feature scaling

The TF-IDF matrix is standardized using `StandardScaler`.

### 4. PCA

PCA is applied with:

```python
PCA(n_components=0.90)
```

The original run reduced the feature space from **700 to 545 components** while retaining 90% of the variance.

### 5. Train/test split

The data is split using:

- Test size: **20%**
- Random state: **42**
- Stratification: enabled

This produced:

- Training set: **14,920 samples**
- Test set: **3,730 samples**

### 6. Classification

Two base models are trained:

- Logistic Regression
- Decision Tree

They are then combined using a `StackingClassifier`, with Logistic Regression as the final estimator and 5-fold internal cross-validation.

## Results

The following results are from the original notebook run.

| Model | Precision | Recall | F1-score |
|---|---:|---:|---:|
| Logistic Regression | 0.9415 | 0.9413 | 0.9414 |
| Decision Tree | 0.8985 | 0.8981 | 0.8983 |
| Stacking Classifier | 0.9430 | 0.9429 | 0.9429 |

### Stacking classification report

| Class | Precision | Recall | F1-score | Support |
|---|---:|---:|---:|---:|
| Phishing Email | 0.92 | 0.93 | 0.93 | 1,466 |
| Safe Email | 0.96 | 0.95 | 0.95 | 2,264 |
| **Weighted Avg.** | **0.94** | **0.94** | **0.94** | **3,730** |

The original notebook also recorded these 5-fold weighted-F1 scores:

```text
[0.9387, 0.9465, 0.9391, 0.9341, 0.9371]
Mean: 0.9391
```

## Repository Structure

```text
email-attack-detection-ensemble-ml/
│
├── Ensemble.ipynb
├── README.md
├── requirements.txt
├── .gitignore
│
├── data/
│   └── README.md
│
└── results/
    └── README.md
```

## Running the Project

### 1. Clone the repository

```bash
git clone https://github.com/YOUR-USERNAME/email-attack-detection-ensemble-ml.git
cd email-attack-detection-ensemble-ml
```

### 2. Install dependencies

```bash
pip install -r requirements.txt
```

### 3. Add the dataset

Download `Phishing_Email.csv` from the dataset source and place it in:

```text
data/Phishing_Email.csv
```

### 4. Open the notebook

```bash
jupyter notebook Ensemble.ipynb
```

You can also upload the notebook to Google Colab after placing the dataset in the expected location.

## Important Reproducibility Note

This repository is a cleaned presentation of the **original academic project implementation**. The original notebook fits TF-IDF, scaling, and PCA before the train/test split, and the recorded cross-validation is performed on the already-transformed full dataset.

That means the reported results should be understood as the results of the original implementation rather than as a leakage-free benchmark.

A future version can improve the experimental design by placing preprocessing inside a scikit-learn `Pipeline` and fitting it separately within each training fold. That would make the evaluation more rigorous, but it would no longer be the exact original experiment.

## Technologies

- Python
- Pandas
- NumPy
- Scikit-learn
- Joblib
- Matplotlib
- Jupyter / Google Colab

## Author

**Ekot Edidiong Emmanuel (Eddy)**

Cybersecurity | Machine Learning | Network & Security Projects
