
# AFL Handball/Kick Classifier

This AFL Handball/Kick Classifier employs a **logistic regression-based approach** for event classification. The process involves extracting meaningful features from match-tracking data, training separate classifiers for **Kick vs. Other events** and **Handball vs. Other events**, and evaluating predictions against both training and unseen test data. The classifiers use acceleration, speed, spin rate, and hang time to distinguish between events.



# **Classification Process**

This algorithm is used to predict whether a `Kick`, `Handball`, or `Other` event has taken place in Australian rules football. The features needed for each ball event are (`accBeforeKick`, `speed`, `spin`, `hangTime`).

The raw data is scaled to have zero mean and unit variance and arranged in a NumPy array. It is first sent through the **Kick vs. Other** classifier, whereupon a label is assigned to the event based on the probability determined by logistic regression.

This probability is computed by comparing the location of the new data in feature space relative to pre-trained decision boundaries. If the probability of a Kick occurring is less than 0.5, an `Other` label is applied; otherwise, it is labeled as a `Kick`. For events labeled `Other`, they are then parsed by the **Handball vs. Other** classifier. Subsequently, an overall confidence value is associated with each event, indicating the likelihood of a correct classification.
   


# **Training and Test Results**
The training and testing data comprised of both professional men's and women's AFL matches. 

The following matches were used to train the classification models:

-   **1711082824802233399**: VFL Round 1 - SAN v COL
 (men)
-   **1731728004506761742**: AFLW Semi-Final 1 - ADEL v FRE  (women)
   
    These matches contained a total of 1190 validated events.

### **Test Match**

The following match was used to evaluate the models on unseen data:

-   **1731136664763788685**: AFLW Elimination Final 1 - FRE v ESS  
(women)    
This match contained 443 validated events. 

## **Kick Vs Other** 
1.  **Training Data Performance**:
    
    -   Achieved **97.90% accuracy** on the training dataset when comparing predicted with ground truth labels
    -   Confusion Matrix:
        -   **True Positives (Kick)**:  544
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


## ***Handball Vs Other*** 

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

### Visualization

Explore the decision boundary for the Kick Vs Other algorithm:
[3D Decision Boundary Visualisation](https://MC4713.github.io/plotly-hosting/3d_decision_boundary.html)
With kicks shown in red and other in blue.
<!--stackedit_data:
eyJoaXN0b3J5IjpbMzEzMjQwMDE3LC02NjczODI4MjAsNzI4OD
M4NTI3LDEyNDU4MjE2OTUsNDg0ODkyMDQxLC0xNzAyNjQwOTk3
LC0xOTQ5Nzc2MTcsLTY1NzQ5MTM4MywtMTM1MTkxMzYyMCwxND
cwODg4NjUsLTEzNjU2OTYyNTIsLTE1OTQxNzY0OTksMTg0NzYy
NDg5MCw1NzAyNzQ3NzIsMTI3MzM5NDg2NCwtMTYzNTI1Nzk2OC
wzODIwNTQ5OTksLTkwMjY2NTg1MF19
-->