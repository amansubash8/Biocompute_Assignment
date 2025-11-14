🚀 Project Pipeline & Rationale
1. Data Exploration — 1_Data_Exploration.ipynb

The real-world .pod5 dataset (control_rep2.pod5) was analyzed.

Determined that the files contain raw signal but lack methylation labels, which would require a complex bioinformatics pipeline (dorado, modkit) for generation.

2. Dataset Creation

Per the assignment’s "toy example" clause, a hybrid dataset was created using:

Real biological noise from control_rep2.pod5 as the base.

Synthetic randomized spikes added to simulate signal patterns in a high-noise environment.

Two datasets were generated:

_ultra (With Gaussian Smoothing):
Gaussian filter (sigma = 0.8) applied to improve signal-to-noise ratio.
➜ Generated via 2_Data_Preprocessing.ipynb

_adv (Without Gaussian Smoothing):
Raw, noisier version for comparison.
➜ Generated via 2_Data_Preprocessing_without_gaussian.ipynb

🧠 Modeling
Baseline Model — 3_Model_Baseline.ipynb

Adaptation of the DeepSignal hybrid concept:
Parallel architecture combining CNN + Bi-LSTM.

Improved Model — 4_Model_Improved.ipynb

A deeper Serial Hybrid:
CNN → CNN → Bi-LSTM

Includes:

BatchNormalization for stable convergence.

Dropout for better generalization and reduced overfitting.

📊 Evaluation
5_Evaluation_Report.ipynb

Models compared on the test dataset.

Generates:

AUC-ROC curves

Confusion matrices

Bar charts of performance metrics (Accuracy, Prec
