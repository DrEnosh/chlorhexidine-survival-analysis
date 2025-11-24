# 🧪 Chlorhexidine Trial – Survival Analysis Using Python  
_An end-to-end time-to-event modeling project based on real ICU clinical data_

![Python](https://img.shields.io/badge/Python-3.10-blue.svg)
![Lifelines](https://img.shields.io/badge/Survival%20Analysis-Lifelines-green.svg)
![Notebook](https://img.shields.io/badge/Jupyter-Notebook-orange.svg)
![Status](https://img.shields.io/badge/Project-Completed-brightgreen.svg)

---

## 📌 Overview

This project performs a complete **survival analysis** on data from a *registered clinical trial* comparing two concentrations of Chlorhexidine (0.12% vs 0.20%) for the prevention of **Ventilator-Associated Pneumonia (VAP)**.

The dataset was provided in raw, unprocessed form with significant missingness and inconsistencies.  
This analysis therefore includes:

- Reconstruction of **survival time**
- Construction of **event status** using a clinically meaningful **hybrid rule**
- Kaplan–Meier estimation  
- Log-rank hypothesis testing  
- Cox proportional hazards regression  

This repository forms part of a Capstone Project in Survival Analysis.

---

## 📂 Repository Structure


---

## 📊 1. Dataset Description

### **A. Baseline Variables**
| Variable | Description |
|---------|-------------|
| Age | Patient's age in years |
| Gender | Male / Female |
| APACHE II Score | Baseline severity-of-illness |
| Trial Arm | Group 1 (CHX 0.12%) / Group 2 (CHX 0.20%) |

### **B. CPIS Measurements**
- CPIS Day 1 → Day 10  
These scores help evaluate infection progression.

### **C. Outcome**
- `Outcome of the current episode`: VAP / No VAP

The dataset does NOT contain survival variables (`time`, `event`) and required custom construction.

---

## ⚙️ 2. Survival Variable Construction (Hybrid Clinical Logic)

Because of inconsistencies in the raw dataset, a hybrid rule was used.

### **Event (`vap_event`) Definition**
A patient is considered to have VAP (`event = 1`) if:

1. Outcome column = **VAP** (confirmed), OR  
2. CPIS ≥ **6** on any day (clinical evidence)

Else → event = **0** (censored).

### **Time (`time`) Definition**
- If CPIS ≥ 6 → first day CPIS reached 6  
- Else → last day CPIS was recorded  
- If no CPIS values → patient removed  

This aligns with clinical decision-making.

---

## 📈 3. Kaplan–Meier Survival Analysis

KM curves were generated for both CHX arms:

- **CHX 0.12%**
- **CHX 0.20%**

These curves show the probability of remaining **VAP-free** over the 10-day follow-up.

Observations:
- Both arms show high survival probabilities  
- Curve separation is minimal  

---

## 🧪 4. Log-Rank Test

A log-rank test compared survival curves between the two groups.

- **Null Hypothesis:** Survival curves are identical  
- **p ≥ 0.05:** No statistical difference  

Given the low event rate, significant differences were unlikely.

---

## 📉 5. Cox Proportional Hazards Model

A multivariable Cox model was fit with:

- Trial arm (binary)  
- Age  
- APACHE II Score  
- Gender  

Outputs include:
- Hazard ratios  
- 95% confidence intervals  
- p-values  
- Forest plot for interpretation  

Interpretation typically showed:
- HR ~ 1 for trial arm → minimal difference  
- APACHE II influence depends on distribution  

---

## ▶️ 6. How to Run This Notebook

### **A. Google Colab**
1. Open Colab  
2. Upload `.ipynb`  
3. Upload the Excel file  
4. Run all cells

### **B. Local**
```bash
pip install lifelines pandas numpy seaborn matplotlib openpyxl
jupyter notebook
# chlorhexidine-survival-analysis
Survival analysis for Chlorhexidine Trial using Kaplan–Meier and Cox model
