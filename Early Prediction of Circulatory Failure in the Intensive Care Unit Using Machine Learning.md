# Early prediction of circulatory failure in the intensive care unit using machine learning

Authors: SL. Harland et al.
Category: Machine Learning
Importance: Foundational
Link: https://www.nature.com/articles/s41591-020-0789-4
Year: 2020

## TL;DR

A supervised machine-learning model based on gradient decision trees serves as an early-warning system for circulatory failure, using measurements from multiple organ systems and patient demographics. It predicts 90% of circulatory-failure events in a test set and 82% up to 2 hours before onset.

## Problem

- Clinicians must analyze large amounts of data from many patients, which can lead to information overload, change blindness, and task fixation. In addition, ICU alarms can contribute to alarm fatigue.
- Many ICU patients experience circulatory failure due to the severity of their illness. Earlier diagnosis and treatment can improve outcomes, including mortality.

## Core Idea

- A machine-learning model ist trained on processed real-world data to detect early development of circulatory failure, a important diagnosis concerning severely ill patients in ICU.

---

## Method / Approach

- A comprehensive dataset (HiRID) was cleaned and processed to allow applicability of machine learning models, including removing artifacts, processing variables and variable merging (attributing).
- **Adaptive imputation** as applied (resampling data to a 5-minute interval) and annotated for each time point (circulatory failure?)
- **Feature extraction from variables** included analyzing tendencies over time, maximum or minimum measurements
    - measurements (MAP, heart rate)
    - Trends (Anstieg/Abfall über Zeit)
    - statistische Aggregationen (Min, Max, Mean)
    - Messhäufigkeit (wie oft wurde etwas gemessen → klinisch relevant!)
- Logistic Regression und LSTM where tested inferior in comparison to GBDT.
- Features and labels are used as input for a **gradient-based decision tree.**
    - gut mit tabellarischen Features umgehen kann
    - Missingness robust verarbeitet
    - nicht so datenhungrig wie LSTM ist
    - Feature Engineering bereits Zeitinformation „einbaut“

## Results

- Dataset: High time resolution ICU dataset (HiRID), containing a total of 7,333 routinely collected physiological variables, diagnostic test results and treatment parameters
from 55,602 patient admissions to the ICU, resulting in more than
3 billion data observations
- Baseline: **Circulatory failure is defined** if (1) arterial lactate is elevated (≥2 mmol l–1), and (2) either mean arterial pressure (MAP) ≤ 65 mmHg, or the patient is receiving vasopressors or inotropes;  decision tree using only the last measurement of the variables included in the circulatory-failure definition (MAP, lactate and dose of vasopressors/
inotropes), mimicking a threshold-based rule system (Logistic Regression).
- Improvement: Two machine-learning based early-warning systems, named circEWS and circEWS-lite, predicted 90% of circulatory-failure events in a test set and 82% up to 2 hours before onset.
- Metrics:
    - **Precision** = fraction of true alarms (true alarms/ total alarms)
    - **Recall** = fraction of captures events (captured events/total events) within 8-hour period before
    - Receiver-operating characteristic curve for the binary classification (AUROC 0,94, higher than baseline)
    - Precision-recall curve (AUPRC 0,6-0,63)
    - high sensitivity/ specificity
- External validation: Test: US ICU (MIMIC-III)

## Problems

- **Labels are clinician decisions, not ground truth.** The target reflects clinical judgment and practice patterns rather than an objective physiological “truth.”
- **Retrospective, predictive analysis only.** The model predicts risk but does not establish a causal mechanism for circulatory failure.
- **Risk of proxy learning / treatment leakage.** Vasopressor/inotrope administration is a clinician intervention and may reflect decision-making, not solely underlying physiology.
- **Potential data leakage from imputation.** Imputation choices can inadvertently introduce information across time or from future observations.
- **Limited generalizability.** Performance may not transfer to other hospitals, patient populations, or measurement practices.
- **Missing prospective/clinical validation.** It remains unclear whether using the system:
    - reduces mortality,
    - reduces ICU complications, or
    - improves clinical decision-making.

## Why it matters

- Analyzation of large data amounts and early prediction of tendencies is essential in treatment of complex diseases and extensively monitored patients.
- Early recognition of pathological patterns allows early intervention and treatment.

## What I understood

1. ICU data are being processed and aggregated to be applicable for machine-learning.
2. Different machine-learning approaches were compared, and gradient-based decision tree was best suited for chaotic data. 

## What I still don’t understand

- Evaluation of circEWS & model performance statistics
