# Overview
This project investigates stress detection using electrocardiogram (ECG) signals from the WESAD dataset.
The workflow includes:  
- ECG signal preprocessing
- Bandpass filtering
- R-peak detection
- Heart Rate Variability (HRV) analysis
- Statistical comparison of stress and baseline states
- Machine learning classification

# Dataset
The study uses the WESAD dataset, a publicly available multimodal dataset for stress and affect detection.
https://www.kaggle.com/datasets/orvile/wesad-wearable-stress-affect-detection-dataset

# Methods
1. ECG signal extraction
2. Signal filtering (0.5-40 Hz bandpass filter)
3. R-peak detection
4. RR interval calculation
5. HRV feature extraction:
   - SDNN
   - RMSSD
   - Mean RR
6. Statistical analysis
7. Random Forest classification

# Results

## Raw electrocardiographic signal recorded from the chest sensor of subject S2.
   - Samples - time samples of the signal.
   - Amplitude - amplitude of the electrical potential.
  The structure of the signal clearly shows the R-peaks of the QRS complex, reflecting ventricular depolarization. Between them,
less pronounced waves (P and T) are observed, corresponding to atrial depolarization and ventricular repolarization.
  The amplitude of the signal varies within approximately −0.4…1.0, which is typical for an unfiltered ECG. There are noise components
and baseline drift, which reduce the accuracy of the analysis and require further bandpass filtering.
<img width="1552" height="477" alt="image" src="https://github.com/user-attachments/assets/c40daa21-ac14-4657-80fe-dc0598e063ae" />

## Filtered electrocardiographic signal 
  obtained after applying a Butterworth bandpass filter with a frequency range of 0.5-40 Hz.
  After processing, the signal acquired a clearer morphology of the QRS complex, the low-frequency baseline drift and high-frequency
noise were reduced. R-peaks remain dominant, which ensures high accuracy of their detection. The signal amplitude stabilized within
-0.3...0.8, which corresponds to the physiological parameters of cardiac activity.
<img width="1553" height="480" alt="image" src="https://github.com/user-attachments/assets/8d42033f-21cd-4d8b-bb88-296138f92304" />

## Filtered electrocardiographic signal with marked detected R-peaks.
  R-peaks were determined using the find_peaks algorithm with the parameter distance=200, which ensures correct separation of neighboring 
heart beats. Each red marker corresponds to the moment of maximum amplitude of the QRS complex - the phase of ventricular depolarization.
The signal demonstrates regular periodicity of R-peaks, which indicates a stable sinus rhythm. The distance between peaks (RR intervals) 
is the basis for further analysis of heart rate variability (HRV), which is used to assess the physiological state and stress level.
<img width="1560" height="482" alt="image" src="https://github.com/user-attachments/assets/8b93ed4a-a1d3-4079-8f6a-6579bb7b1047" />

## The sequence of RR intervals, i.e. the time intervals between adjacent R-peaks of the electrocardiographic signal.
   - Beat Number - the serial number of the heart contraction.
   - Samples - the duration of the RR interval, expressed in the number of signal samples.
  The data demonstrate heart rate variability within approximately 200-500 samples, which corresponds to physiological fluctuations in
heart rate. Despite the density of the graph, small fluctuations in the duration of the intervals are visible, which reflect the dynamic
regulation of the autonomic nervous system.
<img width="1279" height="491" alt="image" src="https://github.com/user-attachments/assets/4c5a01da-73dc-4f79-ba82-7f2b32440be1" />

## Distribution of the SDNN indicator
Classes - 0 (baseline state) and 1 (stress state).
   - label - classification labels of the subject's state.
   - SDNN - standard deviation of RR intervals, which characterizes the overall heart rate variability.
  For class 0, the median SDNN value is about 70, while for class 1 it is about 62. This indicates a decrease in heart rate variability
during stress, which is consistent with the physiological patterns of inhibition of parasympathetic activity.
<img width="851" height="570" alt="image" src="https://github.com/user-attachments/assets/c2738ad1-a6da-4e43-b83b-feacb22d7741" />

## Distribution of the RMSSD indicator
Classes - 0 (baseline state) and 1 (stress state).
   - label - classification labels of the subject's state.
   - RMSSD - root mean square deviation of consecutive RR intervals, which characterizes parasympathetic activity of the heart rate.
  For class 0, the median RMSSD value is about 115, while for class 1 - about 105. This indicates a decrease in short-term heart rate
variability during stress, which corresponds to the physiological inhibition of vagal regulation.
<img width="864" height="578" alt="image" src="https://github.com/user-attachments/assets/cb888fd0-02cb-4e7b-b66f-b6867a0064c9" />

## Distribution of the mean RR interval (MeanRR)
Classes 0 (baseline) and 1 (stressed state).
   - label - classification labels of the subject's state.
   - MeanRR - the mean value of the RR intervals, which characterizes the average duration of the cardiac cycle.
  For class 0, the median value of MeanRR is about 265, and for class 1 — about 270. A slight increase in the mean RR interval in a stressed
state may indicate a decrease in heart rate, which is a compensatory reaction of the body to stress.
<img width="863" height="584" alt="image" src="https://github.com/user-attachments/assets/0fe7c69f-a031-473c-8b1b-4579d074070b" />

## Correlation matrix of heart rate variability (HRV) features, it includes three parameters: SDNN, RMSSD and MeanRR.
The obtained values ​​show:
   - SDNN and RMSSD = 0.94 — a very strong positive correlation, which indicates a joint dependence of these indicators on the total heart rate variability.
   - SDNN and MeanRR = 0.57 — moderate correlation, indicating a partial relationship between total variability and average cardiac cycle duration.
   - RMSSD and MeanRR = 0.64 — moderate positive correlation, demonstrating the influence of short-term rhythm fluctuations on the average RR interval.
<img width="803" height="663" alt="image" src="https://github.com/user-attachments/assets/d997b272-45f8-4cb1-9b98-c0faa9d3f334" />

## The uncertainty matrix 
is ​​used to assess the effectiveness of the classification model, which distinguishes between two states — 0 (baseline) and 1 (stress).
   - X-axis - predicted classes of the model.
   - Y-axis - true classes (true values).
The matrix displays the following classification results:
   - True Negative - 14 cases of correctly identified baseline state.
   - False Positive - 2 cases when the baseline state was incorrectly classified as stress.
   - False Negative - 2 cases when the stress state was classified as baseline.
   - True Positive - 7 cases of correctly identified stressful state.
<img width="613" height="564" alt="image" src="https://github.com/user-attachments/assets/2b4a111b-6c27-40ec-9bfd-9677bfb0f42e" />

## The weight of the features 
obtained after training the Random Forest model for state classification. The importance values ​​were distributed almost evenly:
   - MeanRR = 0.35 is the highest contribution to the classification, which indicates the importance of the average duration of RR intervals for stress recognition.
   - SDNN = 0.34 is the second most influential indicator, reflecting the overall variability of the heart rate.
   - RMSSD = 0.31 is a slightly smaller contribution, but also significant for assessing short-term rhythm fluctuations.
<img width="852" height="557" alt="image" src="https://github.com/user-attachments/assets/cedc54fb-d03a-4a37-9bd5-765ecf8598f3" />

## Class distribution
  Class 0 (Baseline) contains approximately 80 observations, while class 1 (Stress) contains approximately 45. 
This indicates a moderate sample skewness, where the baseline state is represented by a larger number of examples.
<img width="662" height="493" alt="image" src="https://github.com/user-attachments/assets/5b03e07c-0792-4756-973b-ad8a6a5a48b4" />
