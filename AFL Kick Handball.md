
### AFL Handball/Kick Classifier 

The AFL Handball/Kick Classifier employs a **logistic regression-based approach** for event classification. The process involves extracting meaningful features from match-tracking data, training separate classifiers for **kick vs. other events** and **handball vs. other events**, and evaluating predictions against both training and unseen test data. The classifiers use acceleration, speed, spin rate, and hang time to distinguish between events.

----------
### **Process Description**

The classification process for the AFL Handball/Kick Classifier involves a sequential approach to distinguish between kicks, handballs, and other events. Below is a detailed breakdown:

----------

### **Step 1: Kick vs Other Classification**

1.  **Feature Extraction**:
    
    -   The process begins by extracting key features from match-tracking data for each event.
    -   Features such as `accBeforeKick`, `speed`, and `spin` are used to determine if the event is a kick.
2.  **Model Prediction**:
    
    -   The extracted features are fed into the **Kick vs Other Logistic Regression Classifier**.
    -   The classifier outputs:
        -   A predicted label: `Kick` or `Other`
        -   A confidence score for the prediction.
3.  **Decision**:
    
    -   If the event is classified as a `Kick`, the process ends here, and the label `Kick` is assigned.

----------

### **Step 2: Handball vs Other Classification**

1.  **Filtering Non-Kick Events**:
    
    -   If the event is classified as `Other` in the first step, it proceeds to the next stage of classification.
2.  **Feature Extraction**:
    
    -   Additional features such as `hangTime` and `speed` are extracted to differentiate handballs from other non-kick events.
3.  **Model Prediction**:
    
    -   The extracted features are passed to the **Handball vs Other Logistic Regression Classifier**.
    -   The classifier outputs:
        -   A predicted label: `Handball` or `Other`
        -   A confidence score for the prediction.
4.  **Decision**:
    
    -   If the event is classified as `Handball`, the label `Handball` is assigned.
    -   Otherwise, the label `Other` is assigned.

----------

### **Step 3: Final Output**

1.  **Labels**:
    
    -   Each event is assigned one of the three possible labels:
        -   `Kick`
        -   `Handball`
        -   `Other`
2.  **Confidence Scores**:
    -   Alongside the labels, confidence scores for each classification step are provided.
    -   These scores help assess the reliability of the predictions, with a threshold of **0.7** for high-confidence decisions.
3.  **Post-Processing**:
    
 -   Events with low confidence (confidence score below **0.7**) are flagged for further review.
-   For each low-confidence prediction, the **time of the event** and the **predicted output** are aggregated into a separate data frame for subsequent manual inspection.
----------

### **Summary of Process Flow**

1.  **Kick vs Other Classification**:
    -   Directly identifies `Kick` events.
2.  **Handball vs Other Classification**:
    -   Distinguishes `Handball` from other non-kick events.
3.  **Final Output**:
    -   Assigns one of three labels: `Kick`, `Handball`, or `Other`and an associated confidence.
    


## **Performance Evaluation**

 **Kick vs Other Classification**

The **Kick vs Other Classifier** is designed to separate kicks from other events such as handballs and running bounces. The following features are used in classification:

-   **`accBeforeKick`**: Average acceleration before the kick (m/s²).
-   **`speed`**: Ball speed during the event (m/s).
-   **`spin`**: Spin rate of the ball (revolutions per second).

#### **Training and Test Results**

1.  **Training Data Performance**:
    
    -   Achieved **97.90% accuracy** on the training dataset.
    -   Confusion Matrix:
        -   **True Positives (Kick)**: 544
        -   **True Negatives (Other)**: 621
        -   **Misclassifications**: 25 (False Positives: 4, False Negatives: 21)
2.  **Test Data Performance**:
    
    -   Achieved **96.39% accuracy** on unseen test data.
    -   Confusion Matrix:
        -   **True Positives (Kick)**: 169
        -   **True Negatives (Other)**: 258
        -   **Misclassifications**: 16 (False Positives: 6, False Negatives: 10)


![Kick vs Other - Training Confusion Matrix](https://i.imgur.com/Y1ErSA8.png)

![Kick vs Other - Test Confusion Matrix](https://i.imgur.com/fuQprIk.png)

----------

### **Handball vs Other Classification**

The **Handball vs Other Classifier** identifies handball events among non-kick events. Key features for classification include:

-   **`hangTime`**: Duration the ball remains airborne (seconds).
-   **`speed`**: Ball speed during the event (m/s).

#### **Training and Test Results**

1.  **Training Data Performance**:
    
    -   Achieved **98.40% accuracy** on the training dataset.
    -   Confusion Matrix:
        -   **True Positives (Handball)**: 424
        -   **True Negatives (Other)**: 191
        -   **Misclassifications**: 10 (False Positives: 5, False Negatives: 5)
2.  **Test Data Performance**:
    
    -   Achieved **98.11% accuracy** on unseen test data.
    -   Confusion Matrix:
        -   **True Positives (Handball)**: 169
        -   **True Negatives (Other)**: 90
        -   **Misclassifications**: 5 (False Positives: 3, False Negatives: 2)


![Handball vs Other - Training Confusion Matrix](https://i.imgur.com/9fO7xsy.png)

![Handball vs Other - Test Confusion Matrix](https://i.imgur.com/4vGrL7r.png)

----------

### **Match Data**

#### **Training Matches**

The following matches were used to train the classification models:

-   **1711082824802233399**: VFL Round 1 - SAN v COL
-   **1731728004506761742**: AFLW Semi-Final 1 - ADEL v FRE
- These matches resulted in a total of 1190 validated events

#### **Test Match**

The following match was used to evaluate the models on unseen data:

-   **1731136664763788685**: AFLW Elimination Final 1 - FRE v ESS
- This match contained 443 validated events
<!--stackedit_data:
eyJoaXN0b3J5IjpbLTE2MzUyNTc5NjgsMzgyMDU0OTk5LC05MD
I2NjU4NTBdfQ==
-->