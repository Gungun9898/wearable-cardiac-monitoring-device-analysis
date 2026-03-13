# wearable-cardiac-monitoring-device-analysis

# From Data to Diagnosis: Systematic and Bibliometric Analysis of ML-Based Wearable Cardiac Monitoring

> MBA Summer Internship Project | ICFAI Business School Bangalore | Technomers | May 2025

---

## Overview

This project investigates the role of **machine learning in early detection and monitoring of cardiovascular diseases (CVD)** using physiological data from wearable devices. It combines a **systematic literature review (SLR)**, **bibliometric analysis**, and **hands-on ML model development** to identify the most effective algorithms for cardiac health monitoring.

The work was conducted as part of an MBA internship at **Technomers, Mangaluru** — a product engineering and consulting firm specializing in AI/ML, cloud, and health-tech solutions — in support of their client **Movano**, a smart wearable device company.

---

## Project Objectives

- Review and synthesize existing research on ML-based wearable cardiac monitoring using the Scopus database
- Perform bibliometric analysis to identify dominant algorithms, authors, and research trends
- Conduct exploratory data analysis on the WESAD dataset using physiological signals (ECG, EDA, respiration)
- Build and evaluate ML models (Random Forest, XGBoost, SVM) for personalized activity type detection
- Provide evidence-based recommendations for Technomers' client on optimal ML techniques

---

## Methodology

### 1. Systematic Literature Review (SLR)
- **Database:** Scopus (Elsevier)
- **Search terms:** wearable devices, machine learning, cardiac, heart, AI, XGBoost, random forest, SVM
- **Results:** 884 articles screened → 71 included → **18 articles fully matched** the research focus
- **Framework:** PRISMA (Preferred Reporting Items for Systematic Reviews and Meta-Analyses)

### 2. Bibliometric Analysis
- Tool: **VOSviewer**
- A custom Python script converted Scopus Excel data (titles, authors, keywords, abstracts) into CSV format
- Generated: keyword co-occurrence maps, author collaboration networks, abstract/title term analysis

### 3. Exploratory Data Analysis
- **Dataset:** WESAD (Wearable Stress and Affect Detection) from UCI Machine Learning Repository
- **Device data used:** RespiBAN (Patient S2) and Empatica E4 (Patient S4)
- **Signals analysed:** ECG, EDA, EMG, TEMP, XYZ accelerometer, Respiration
- Signal preprocessing: centering, downsampling (700 Hz → 35 Hz), bandpass filtering (0.05–0.8 Hz)
- Generated recurrence plots and breathing feature extraction

### 4. ML Model Development
- **Features:** Mean and standard deviation of HR and EDA (30-second windows)
- **Models:** Random Forest, XGBoost, Support Vector Machine (SVM)
- **Labels:** 0 = Rest, 1 = Mental task, 2 = Physical activity
- **Split:** 80% training / 20% testing

---

## Key Findings

### Literature Review
- **Random Forest** and **XGBoost** were the most widely used and highest-performing algorithms across reviewed studies
- Wearable ML systems showed strong results for detecting atrial fibrillation, stress, and CVD risk

### Respiration Signal Analysis (Patient S2 — RespiBAN)
| Feature | Value |
|---|---|
| Breathing rate | 21.26 breaths/min (slightly elevated; normal: 12–18) |
| Mean amplitude | 19,546.81 ADC units |
| Amplitude std | 1,717.36 |
| Mean breath-to-breath interval | 2.82 seconds |
| BBI variability | 0.78 seconds |
| Breaths detected | 2,249 |
| Recurrence Rate | 0.20 |
| Determinism | 1.0 (100% predictable sequences) |

The recurrence plot showed a **highly structured, self-similar, grid-like pattern** — indicating a calm, non-stressed physiological state.

### Model Comparison (Activity Type Detection)

| Metric | SVM | XGBoost | Random Forest |
|---|---|---|---|
| Accuracy | 0.26 | **0.26** | 0.15 |
| Macro Avg Precision | 0.09 | **0.27** | 0.17 |
| Macro Avg Recall | 0.33 | **0.28** | 0.14 |
| Macro Avg F1-Score | 0.14 | **0.26** | 0.15 |
| Weighted Avg F1-Score | 0.11 | **0.25** | 0.16 |

**XGBoost outperformed both SVM and Random Forest** across nearly all metrics. SVM predicted only one class effectively. Random Forest had the lowest overall accuracy.

> Note: Low accuracy across all models is expected given the small dataset size, simulated labels, and limited features (mean + std of EDA and HR only). This was an exploratory analysis — see Limitations.

### Feature Importance (XGBoost & Random Forest)
- **EDA standard deviation** was the most important feature in both models
- Variability in skin conductance is a stronger indicator of activity state than its mean value
- HR standard deviation also contributed meaningfully to class separation

---

## Tech Stack

| Category | Tools / Libraries |
|---|---|
| Language | Python |
| Data manipulation | pandas, numpy |
| Signal processing | scipy (butter, filtfilt, find_peaks) |
| Visualisation | matplotlib, seaborn |
| ML models | scikit-learn (RandomForestClassifier, SVC), xgboost |
| Recurrence analysis | pyts (RecurrencePlot) |
| Bibliometric analysis | VOSviewer |
| Literature database | Scopus (Elsevier) |
| IDE | Visual Studio |

---

## Repository Structure

```
wearable-cardiac-monitoring-analysis/
│
├── README.md
│
├── bibliometric_analysis/
│   ├── scopus_to_csv.py              # Converts Scopus Excel export to VOSviewer CSV
│   ├── vosviewer_keywords.txt        # Keyword co-occurrence output
│   ├── vosviewer_authors.txt         # Author network output
│   └── vosviewer_abstract_terms.txt  # Abstract term analysis output
│
├── signal_analysis/
│   ├── respiration_signal.py         # Downsampling, bandpass filter, respiration plot
│   ├── recurrence_plot.py            # Recurrence plot + feature extraction (RR, DET)
│   └── breathing_features.py        # Segment-wise breathing feature comparison
│
├── activity_detection/
│   ├── activity_detection_rf_xgb.py  # Random Forest + XGBoost classification
│   ├── activity_detection_svm.py     # SVM classification
│   └── feature_importance.py        # Feature importance visualisation
│
├── data/
│   └── README_data.md               # Instructions to download WESAD dataset from UCI
│
└── outputs/
    ├── recurrence_plot.png
    ├── rf_classification_report.png
    ├── xgb_classification_report.png
    ├── confusion_matrices/
    └── feature_importance_graphs/
```

---

## Dataset

The WESAD dataset is publicly available from the [UCI Machine Learning Repository](https://archive.ics.uci.edu/dataset/465/wesad+wearable+stress+and+affect+detection).

- **RespiBAN device data** (Patient S2): ECG, EDA, EMG, TEMP, XYZ, Respiration at 700 Hz
- **Empatica E4 device data** (Patient S4): HR.csv, EDA.csv

> Data files are not included in this repository due to size. Download from UCI and place in the `data/` folder before running scripts.

---

## Limitations

- Dataset size was small and labels were **simulated** (no ground-truth activity annotations available for the specific signals used)
- Only basic statistical features (mean, std) were used — more advanced features (frequency-domain, peak detection) would likely improve accuracy
- Models were evaluated on a single patient's data, limiting generalisability
- Class imbalance in the test set affected SVM and Random Forest performance significantly

---

## Future Work

- Apply **LSTM or CNN** models better suited for time-series biosignal data
- Incorporate **hyperparameter tuning** and **cross-validation**
- Use **larger, balanced, annotated datasets** with real activity labels
- Explore **multi-sensor fusion** combining ECG, EDA, temperature, and accelerometer signals
- Involve healthcare professionals in clinical validation of model outputs

---

## Acknowledgements

- **Dr. Roshny Unnikrishnan** — Faculty Guide, ICFAI Business School Bangalore
- **Mrs. Sadhvi Gurudatha** — Industry Guide, Technomers
- ICFAI Business School, Bangalore (Off-campus centre of IFHE)
- Technomers, Mangaluru — for the internship opportunity and project scope

---

## Author

**Gunjan Sahoo**
MBA — IT Analytics & Marketing | ICFAI Business School, Bangalore
Enrolment No.: 24BSOCBL0572
[LinkedIn](https://linkedin.com/in/gunjan-sahoo) | gunjsa99@gmail.com
