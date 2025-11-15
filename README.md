# BioCompute ML Assignment: 1D Signal Classification

This repository contains the full implementation for the **BioCompute Machine Learning Engineer Assignment**, including data exploration, synthetic dataset creation, signal preprocessing, model development, and evaluation.  
The main goal of the project was to **adapt the DeepSignal model for 1D nanopore signal classification** and then design an improved architecture for higher accuracy.

---

## Project Overview

### Objective
To classify 1D nanopore signals by adapting the DeepSignal architecture and improving it using a deeper hybrid neural network.

### Key Decisions
The provided `.pod5` data (`control_rep2.pod5`) contains **raw nanopore signal** but **no methylation labels**.  
Generating such labels requires a full bioinformatics pipeline (dorado + modkit), which is outside the assignment scope.

To meet the requirements, a **hybrid synthetic dataset** was created:

- Real biological noise from `control_rep2.pod5`  
- Randomized synthetic signal spike injected  
- Binary labels for a controlled but high-noise classification task  

---

## Project Pipeline

### 1. Data Exploration  
Notebook: `1_Data_Exploration.ipynb`

- Inspected `.fast5` structure
- Inspected `.pod5` structure  
- Verified raw signal presence  
- Confirmed absence of methylation labels → synthetic labels required  

---

### 2. Dataset Creation & Preprocessing  
Two datasets were created:

#### **ultra (recommended)**
- Gaussian smoothing applied (`sigma = 0.8`)  
- Higher signal-to-noise ratio  
- Used for final high-accuracy results  
- Generated using: `2_Data_Preprocessing.ipynb`

#### **adv (no smoothing)**
- No smoothing  
- High-frequency noise retained  
- More challenging for models  
- Generated using: `2_Data_Preprocessing_without_gaussian.ipynb`

Both pipelines output:
- `X_train_*.npy`
- `y_train_*.npy`
- `X_test_*.npy`
- `y_test_*.npy`

The '*' is replaced by either adv or ultra, depending on the data preprocessing done.

---

## 3. Baseline Model  
Notebook: `3_Model_Baseline.ipynb`

A simplified DeepSignal-style architecture:

- Parallel **CNN** and **Bi-LSTM** branches  
- Feature merging in dense layers  
- Acts as the baseline for comparison  

---

## 4. Improved Model  
Notebook: `4_Model_Improved.ipynb`

A deeper **serial hybrid model**:

- Two stacked CNN layers  
- Bi-LSTM for sequential context  
- BatchNormalization + Dropout for stability  
- Achieves **≈99% accuracy on _ultra_ dataset**

---

## 5. Evaluation  
Notebook: `5_Evaluation_Report.ipynb`

Generates:

- ROC curves  
- Confusion matrices  
- Precision/Recall/F1 metrics  
- Side-by-side model comparison  

Models evaluated:
- `baseline_model.keras`
- `improved_model_ultra.keras`

---
## Environment Setup

### Requirements
- Python **3.10.17+**  
- `control_rep2.pod5` (from `modbase-validation_2024.10` dataset)

---

## Installation Steps

# 1. Create virtual environment
python3 -m venv venv

# 2. Activate environment
# macOS/Linux:
source venv/bin/activate
# Windows:
.\venv\Scripts\activate

# 3. Install dependencies
pip install -r requirements.txt

---
## Reproducing the Final Results (Recommended)

### **Step 1: Prepare the Data**
1. Place `control_rep2.pod5` in the project root directory.  
2. Run **2_Data_Preprocessing.ipynb** to generate the following files:

- `X_train_ultra.npy`
- `y_train_ultra.npy`
- `X_test_ultra.npy`
- `y_test_ultra.npy`

---

### **Step 2: Train the Baseline Model**
Run **3_Model_Baseline.ipynb**.  
This notebook loads the `_ultra` dataset and saves:

baseline_model.keras


---

### **Step 3: Train the Improved Model**
Run **4_Model_Improved.ipynb** for 50 epochs.  
This will save the improved model:

improved_model_ultra.keras

---

### **Step 4: Generate the Final Report**
Run **5_Evaluation_Report.ipynb** to generate:

- ROC curves  
- Confusion matrices  
- Final performance metric visualizations  

---

## Optional: Reproduce the “No Gaussian Smoothing” Results

### **Step 1: Generate Noisy Data**
Run **2_Data_Preprocessing_without_gaussian.ipynb** to create the `_adv` dataset files.

---

### **Step 2: Update the SAVE_PREFIX**
In notebooks **3**, **4**, and **5**, modify the first cell:

```python
SAVE_PREFIX = "_ultra"
```


