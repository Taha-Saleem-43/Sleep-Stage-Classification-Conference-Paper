# Sleep Stage Classification from PPG-Derived Heart Rate and Wrist Motion

## Overview
This project presents a **wearable-friendly sleep stage classification pipeline** using **PPG-derived heart rate** and **wrist motion data** collected from Apple Watch devices. The goal is to achieve an effective balance between **classification performance** and **computational efficiency**, making the approach suitable for **on-device or near-device deployment**.

The system uses lightweight feature engineering, multi-scale temporal context, a **histogram-based gradient boosting classifier**, and **Hidden Markov Model (HMM) smoothing** to enforce physiologically plausible sleep-stage transitions.

---

## Dataset
- **Source:** PhysioNet Sleep-Accel Dataset  
- **Signals Used:**
  - PPG-derived heart rate (bpm)
  - Tri-axial wrist acceleration (g)
  - Step counts
- **Labels:** PSG-scored sleep stages  
- **Epoch Length:** 30 seconds (non-overlapping)

### Sleep Stage Resolutions
- **5-stage:** Wake, N1, N2, N3, REM  
- **4-stage:** Wake, Light (N1+N2), Deep (N3), REM  
- **3-stage:** Wake, NREM (N1+N2+N3), REM  

---

## Feature Engineering
### Motion Features
- Vector magnitude (mean, std, max)
- Axis-wise mean and standard deviation
- ENMO (Euclidean Norm Minus One)
- Sample count per epoch

### Heart Rate & HRV-Proxy Features
- Mean, std, min, max HR
- Inter-beat interval (IBI) statistics
- RMSSD-like variability measures
- HR sample count per epoch

### Additional Features
- Step count summaries
- Time-of-night encoding using sine/cosine transforms
- Binary indicators for missing heart rate data

---

## Exploratory Data Analysis
- Class imbalance analysis (notably minority class N1)
- Feature correlation (Spearman & Pearson)
- Principal Component Analysis (PCA)
- ANOVA and effect-size analysis (η², ω²)
- Mutual information-based feature importance
- Sleep-stage transition dynamics and entropy analysis

---

## Modeling Approach
### Temporal Context Augmentation
Rolling window statistics computed over:
- 5, 10, 20, and 40 epochs (2.5–20 minutes)

### Classifier
- **Histogram-based Gradient Boosting**
- Depth-limited trees
- Inverse-frequency class weighting
- Subject-wise train/test split (80/20)

### Sequence Smoothing
- **Hidden Markov Model (HMM)**
- Transition probabilities learned from training subjects
- Viterbi decoding using classifier probability outputs

---

## Evaluation Metrics
- Accuracy
- Macro-averaged F1 score
- Cohen’s Kappa (κ)
- Confusion matrices and per-class F1

---

## Results (Held-Out Subjects)
| Task | Method | Accuracy |
|-----|-------|----------|
| 5-stage | Raw | 0.516 |
| 5-stage | HMM | **0.526** |
| 4-stage | Raw | 0.559 |
| 4-stage | HMM | **0.560** |
| 3-stage | Raw | 0.699 |
| 3-stage | HMM | **0.707** |

HMM smoothing improves temporal consistency and yields modest accuracy gains, particularly for coarser stage resolutions.

---

## Key Insights
- Motion variability features dominate discriminative power
- Heart rate variability helps differentiate wake vs sleep
- N1 and N3 remain challenging due to signal limitations
- HMM smoothing improves hypnogram plausibility

---

## Limitations
- Reduced sensitivity for minority stages (N1, N3)
- No raw PPG waveform or respiratory signals
- Performance constrained by wearable sensor fidelity

---

## Conclusion
This project demonstrates that **lightweight machine learning models combined with probabilistic temporal smoothing** can provide effective sleep staging using wearable data. The approach offers a strong, interpretable baseline for **resource-constrained sleep analytics**.

---

## Authors
- **Ch. Muhammad Ahsan**
- **Taha Saleem**
- **Fahad Rahman**
- **Abdul Rahman Tahir**
- **Faisal Bukhari**

Department of Data Science  
Faculty of Computing and Information Technology  
University of the Punjab, Lahore

---

## References
PhysioNet Sleep-Accel Dataset and related literature on wearable sleep staging, gradient boosting, and Hidden Markov Models.
