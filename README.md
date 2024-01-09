# ML-Driven Cardiac Arrhythmia Detection

Authored by Yihang Du, Binjie Li, Ethan Guo, Jason Jewell.


## Objective

Our objective is to use features (intervals, wavelength, axis, etc) extracted from ECG signals from healthy and arrhythmia patients to perform classification and predict diagnostic results. Our primary goal is to distinguish and identify healthy heart rhythms from abnormal rhythms.

## Data

Our data is acquired from Kaggle, which encompasses 10646 patients’ ECG records. The dataset consist of 13 features: PatientAge, Gender, VentricularRate, AtrialRate, QRSDuration, QTInterval, QTCorrected, RAxis, TAxis, QRSCount, QOnset, QOffset, TOffset. It has two columns of labels: Rhythm and Beat. Rhythm roughly classifies ECGs into 11 classes based on physicians’ judgment. Beat gives more specific diagnostic classes (more than 50). 

## Pre-processing: 

We combined Beat and Rhythm to form the target variable for the first data set we used for prediction. As SA and SB rhythm types combined with NONE beat would denote a normal heartbeat with no disease, we encoded the label for these examples to 0 and 1 otherwise. The encoded target variable replaced the values of the original Beat column. Other unnecessary features were all dropped, such as FileName and Gender. PatientAge is not one of the ECG features but was kept because it usually correlates to diagnosing a patient’s health condition. All data was normalized. The training, validating, and testing sets were split with a ratio of 6:2:2.

## Model

KNN - We use K-nearest neighbor (KNN) with Euclidian distance as the distance metric due to model's inherent robustness to noise and efficient adaptation to training
C4.5 - This is basically an extension of ID3. The primary difference is that C4.5 can handle continuous variables, which is what we need for this project. We implemented the algorithm by ourselves with reduced error pruning.
J48 - This is essentially the same as C4.5 but assembled in Weka. Its implementation of C4.5 would be more mature than our own version.
FNN - We constructed a simple Feedforward Neural Network (FNN) model and put more effort into tuning hyper-parameters.

## Results


|           | KNN  | J48  | C4.5 | FNN  |
|-----------|------|------|------|------|
| Accuracy  | 0.94 | 0.97 | 0.96 | 0.95 |
| Recall    | 0.94 | 0.97 | 0.95 | 0.95 |
| Precision | 0.93 | 0.97 | 0.96 | 0.95 |

All models are evaluated on the test set randomly sampled from the data set



## Analysis
High Overall Accuracy: All models (KNN, J48, C4.5, FNN) exhibit high accuracy, with J48 leading at 0.97, followed by C4.5 at 0.96, KNN at 0.94, and FNN at 0.95. This indicates strong performance in correctly identifying both normal and arrhythmic heartbeats.

J48 as the Most Effective Model: The J48 model, an implementation of C4.5 in Weka, outperforms the others in all metrics (accuracy, recall, precision), suggesting that its mature implementation and better handling of the dataset's nuances contribute to superior performance.

KNN's Robustness Against Noise: Despite KNN's slightly lower metrics, its robustness against noise and adaptability to training data make it a reliable choice, particularly in high data variability.

High Recall in J48, C4.5, and FNN: The high recall values for J48 (0.97), C4.5 (0.95), and FNN (0.95) indicate a low false-negative rate, which is crucial in medical diagnoses where missing an abnormal case (arrhythmia) could be critical.

Precision and Implications for Clinical Use: The high precision of J48 and C4.5 means fewer false positives, which is essential to avoid unnecessary anxiety or treatment for patients. KNN, while slightly lower in precision, still maintains a commendable level, indicating its usefulness in scenarios where model interpretability and simplicity are prioritized.

An interesting observation is that the reduced error pruning technique remarkably improved the performance of C4.5, as the overall accuracy was no higher than 0.60 before pruning the tree. The general performance of our implementation of C4.5 can compete with, even though not win out, J48. However, our version of C4.5 might not be as robust as J48 as it just naively splits variables into binary.


## Files and Data

- Diagnostics.xlsx
Original dataset. Rhythm column classifies the patterns of ECG. Beat column denotes the diagnosed disease. Since for SB rhythm, it is hard to tell if a patient is health or not, we try to use machine learning to classify patients with SB rhythm as healthy (N for negative) and unhealthy (P for positive).

- Diagnostics.csv
Same as Diagnostics.xlsx but in .csv format.

- Diagnostics_processed.csv
Unnecessary columns (FileName, Rhythm, PatientAge, and Gender) are dropped. Only SB rhythm samples are kept. Re-encoding Beat to N and P; NONE is encoded to N and others are encoded to P. All data are normalized.

- Diagnostics_train.csv, Diagnostics_valid.csv, Diagnostics_test.csv
These three files are split from shuffled Diagnostics_processed.csv with the ratio of 6:2:2.

- Diagnostics_preprocessing.ipynb
Some simple preprocessing code to do what we have described above. If un-normalized data is needed, simply comment out the for-loop that normalizes the data and re-run all blocks.

- ForWeka
The directory contains scripts and files needed to do prediction with Weka. For most of algorithms assembled in Weka, we have 69% overall accuracy.