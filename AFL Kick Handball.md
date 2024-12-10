

## AFL Handball/Kick Classifier

This AFL Handball/Kick Classifier employs a **logistic regression-based approach** for event classification. The process involves extracting meaningful features from match-tracking data, training separate classifiers for **kick vs. other events** and **handball vs. other events**, and evaluating predictions against both training and unseen test data. The classifiers use acceleration, speed, spin rate, and hang time to distinguish between events.

-------

### **Classification Process**

1.  **Kick vs Other**:
    
    -   Features (`accBeforeKick`, `speed`, `spin`) are used by the **Kick vs Other Classifier** to predict `Kick` or `Other`.
    -   If classified as `Kick`, the label `Kick` is assigned.
2.  **Handball vs Other**:
    
    -   Non-kick events are processed by the **Handball vs Other Classifier** using features (`hangTime`, `speed`) to predict `Handball` or `Other`.
    -   Labels `Handball` or `Other` are assigned accordingly.
3.  **Low Confidence Handling**:
    
    -   Predictions with confidence scores below **0.7** are flagged.
    -   These events, along with their times and predicted labels, are stored in a separate data frame for manual review.
----------
# **Kick vs Other Classification**

The **Kick vs Other Classifier** is designed to separate kicks from other events such as handballs, running bounces and ball ups. The following features are used in classification:

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

# **Handball vs Other Classification**

The **Handball vs Other Classifier** identifies handball events among non-kick events. Required features:

-   **`hangTime`**: Duration the ball remains airborne (seconds).
-   **`speed`**: Ball speed during the event (m/s).

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

# **Match Data**

#### **Training Matches**

The following matches were used to train the classification models:

-   **1711082824802233399**: VFL Round 1 - SAN v COL
-   **1731728004506761742**: AFLW Semi-Final 1 - ADEL v FRE
These matches contained a total of 1190 validated events

#### **Test Match**

The following match was used to evaluate the models on unseen data:

-   **1731136664763788685**: AFLW Elimination Final 1 - FRE v ESS
This match contained 443 validated events

The following link will allow you to visualise the kick vs other decision boundary in 3d:

https://MC4713.github.io/plotly-hosting/3d_decision_boundary.html

<!--stackedit_data:
eyJoaXN0b3J5IjpbLTEzNjU2OTYyNTIsLTE1OTQxNzY0OTksMT
g0NzYyNDg5MCw1NzAyNzQ3NzIsMTI3MzM5NDg2NCwtMTYzNTI1
Nzk2OCwzODIwNTQ5OTksLTkwMjY2NTg1MF19
-->