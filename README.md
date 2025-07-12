# Automated Sleep Stage Classification using Machine Learning

## 📜 Table of Contents
- [Introduction](#introduction)  
- [Motivation & Objectives](#motivation--objectives)  
- [Dataset](#dataset)  
- [Data Preprocessing](#data-preprocessing)  
- [Epoch Creation & Event Markers](#epoch-creation--event-markers)  
- [Theoretical Background](#theoretical-background)  
- [Methodology](#methodology)  
- [Machine Learning Models](#machine-learning-models)  
- [Model Optimization & Ensemble Learning](#model-optimization--ensemble-learning)  
- [Evaluation Metrics](#evaluation-metrics)  
- [Results & Conclusion](#results--conclusion)  
- [References](#references)  
- [Author](#author)  

---

## Introduction

Sleep staging is a fundamental process in sleep medicine, crucial for diagnosing sleep disorders such as insomnia, sleep apnea, and narcolepsy. Manual scoring of sleep stages, based on polysomnography (PSG) recordings, is labor-intensive, time-consuming, and susceptible to inter-scorer variability. This project aims to automate the classification of sleep stages—Wake (W), N1, N2, N3, and REM—using EEG signals recorded in overnight PSG studies.

Using machine learning techniques, we leverage the Sleep-EDF dataset, which provides rich EEG and annotation data, to train classifiers capable of accurately and efficiently segmenting and classifying sleep into clinically relevant stages.

---

## Motivation & Objectives

### Motivation  
The manual annotation of sleep stages requires expert clinicians and is constrained by scalability and consistency. Automating sleep staging could enable broader, faster, and more cost-effective sleep diagnostics and research.

### Objectives  
- Develop a reliable automated pipeline to classify EEG signals into five sleep stages.  
- Utilize classical and ensemble machine learning models to improve classification robustness.  
- Evaluate the system rigorously using standard sleep scoring metrics to ensure clinical relevance.  
- Achieve classification accuracy above 80% as a baseline for practical use.

---

## Dataset

- **Source:** [PhysioNet Sleep-EDF Database](https://physionet.org/content/sleep-edfx/1.0.0/)  
- **Contents:**  
  - PSG `.edf` files containing whole-night polysomnographic recordings including EEG (Fpz-Cz and Pz-Oz channels), EOG, EMG, respiration, and other physiological signals.  
  - Corresponding hypnogram annotation files manually scored according to Rechtschaffen and Kales (1968) standards.  
- **Labels:** Wake (W), N1, N2, N3, REM, Movement Time (M), and undefined stages. For this project, the focus is on the standard five-stage classification.

---

## Data Preprocessing

### EEG Signal Extraction  
- Only EEG signals from Fpz-Cz and Pz-Oz electrode placements are retained to focus on brain activity related to sleep stages.  
- Other physiological signals (EOG, EMG, respiration, temperature) are discarded to reduce noise and dimensionality.

### File Organization  
- PSG and Hypnogram files are paired by subject ID to maintain data integrity.  
- Wake periods preceding and following sleep sessions are cropped (typically 30 minutes) to focus analysis on actual sleep.

### Signal Cleaning & Transformation  
- EEG signals are loaded using the MNE-Python library, ensuring standardization and compatibility.  
- Annotations from hypnograms are mapped onto raw EEG signals for supervised learning.

---

## Epoch Creation & Event Markers

- EEG signals are segmented into uniform 30-second epochs, consistent with American Academy of Sleep Medicine (AASM) standards.  
- Custom event generation aligns sleep stage labels with epochs, ensuring each segment accurately reflects the annotated stage.  
- Epochs with uncertain or undefined labels are excluded to maintain data quality.

---

## Theoretical Background

### Sleep Staging Fundamentals  
Sleep is classified into discrete stages based on EEG patterns:  
- **Wake (W):** Characterized by alpha and beta waves indicating alertness.  
- **N1:** Light sleep stage marked by theta waves and slow eye movements.  
- **N2:** Defined by sleep spindles and K-complexes in EEG.  
- **N3:** Deep sleep or slow-wave sleep characterized by delta waves.  
- **REM:** Rapid eye movement sleep with low amplitude mixed frequency EEG and muscle atonia.

### Machine Learning in Sleep Staging  
Classifying sleep stages is a challenging multiclass problem due to the non-stationary, noisy nature of EEG signals and inter-subject variability. Traditional algorithms rely on handcrafted features extracted from frequency, time, and nonlinear domains. Ensemble learning methods help improve prediction stability and accuracy by combining multiple models.

---

## Methodology

### Feature Extraction  
- EEG epochs are transformed into feature vectors capturing temporal, spectral, and statistical characteristics (e.g., power spectral densities, wavelet coefficients, Hjorth parameters).  
- These features serve as inputs for downstream machine learning classifiers.

### Classification Pipeline  
- Data is split into training, validation, and test sets ensuring no subject overlap.  
- Models are trained using cross-validation to optimize hyperparameters and reduce overfitting.  
- Ensemble methods are applied to combine strengths of individual models.

---

## Machine Learning Models

- **Decision Tree:** Interpretable model splitting features based on entropy to predict sleep stages.  
- **Support Vector Machine (SVM):** Utilizes the RBF kernel to handle complex nonlinear patterns in feature space.  
- **Random Forest:** Aggregates multiple decision trees trained on bootstrapped data to improve generalization.  
- **XGBoost:** Gradient boosting technique with regularization to minimize overfitting and maximize accuracy.

---

## Model Optimization & Ensemble Learning

### Hyperparameter Tuning  
- **Grid Search:** Exhaustive search across parameter grid for best combinations.  
- **Randomized Search:** Random sampling of hyperparameter space for efficiency.

### Ensemble Strategies  
- **Voting Classifier:** Combines SVM, Random Forest, and XGBoost using hard and soft voting to aggregate predictions.  
- **Bagging:** Training multiple classifiers on different random subsets to reduce variance.  
- **Boosting:** Sequentially trains models emphasizing misclassified samples for improved accuracy.

---

## Evaluation Metrics

- **Accuracy:** Overall proportion of correct predictions.  
- **Confusion Matrix:** Detailed class-wise prediction analysis.  
- **Precision, Recall, F1-score:** Class-specific performance indicators.  
- **AUC-ROC Curve:** Model discrimination ability across different thresholds.

---

## Results & Conclusion

- Both SVM and XGBoost models demonstrated high individual performance, effectively capturing the EEG feature complexities.  
- Ensemble classifiers, particularly with soft voting, achieved superior accuracy and robustness compared to single models.  
- AUC-ROC analysis validated improved predictive power of ensemble methods.  
- The pipeline provides a scalable and automated solution for clinical sleep stage classification, reducing reliance on manual scoring.

---

## References

- PhysioNet Sleep-EDF Database: https://physionet.org/content/sleep-edfx/1.0.0/  
- Rechtschaffen and Kales, A Manual of Standardized Terminology, Techniques, and Scoring System for Sleep Stages of Human Subjects (1968).  
- MNE-Python: https://mne.tools/stable/index.html  
- Chen, T. & Guestrin, C. “XGBoost: A Scalable Tree Boosting System”, KDD 2016.

---

## Author

**Rudra Vyas**  
🔗 GitHub: [@Rudravyas233](https://github.com/Rudravyas233)  
