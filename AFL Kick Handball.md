
# AFL Handball/Kick Classifier

This AFL Handball/Kick Classifier employs a **logistic regression-based approach** for event classification. The process involves extracting meaningful features from match-tracking data, training separate classifiers for **kick vs. other events** and **handball vs. other events**, and evaluating predictions against both training and unseen test data. The classifiers use acceleration, speed, spinRate, and hangTime to distinguish between events.

----------

### **Classification Process**
 
 This classifier is used to predict whether a `Kick`, 'handball' or `Other` event has taken place in Australian rules football. The features needed for each ball event are (`accBeforeKick`, `speed`, `spin`, 'hangTime'). 

The raw data is scaled to have zero mean and unit variance and arranged in a NumPy array. It is first sent through the Kick other classifier whereupon a label is assigned to the event based on the probability denoted by logitstic regression. 

This is computed by comparing the  location of new data in feature space with respect to some pre-trained decision boundaries. If the probability of a kick occuring is less than 0.5, an 'other' label is applied, else it is labeled as a kick. For events labeled other, they are then parsed by the handball other classifier. Subsequently, an overall confidence
value is associated to each event indicating the belief about the liklihood of a correct classification. 
   
----------

#### **Training and Test Results**
The training and testing data comprised of both prossional men's and women's AFL matches. 

1.  **Training Data Performance**:
    
    -   Achieved **97.90% accuracy** on the training dataset.
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

### Visualization

Explore the decision boundary:  
[3D Decision Boundary Visualisation](https://MC4713.github.io/plotly-hosting/3d_decision_boundary.html)
<!--stackedit_data:
eyJoaXN0b3J5IjpbMTA1ODU4NzMwNyw0ODQ4OTIwNDEsLTE3MD
I2NDA5OTcsLTE5NDk3NzYxNywtNjU3NDkxMzgzLC0xMzUxOTEz
NjIwLDE0NzA4ODg2NSwtMTM2NTY5NjI1MiwtMTU5NDE3NjQ5OS
wxODQ3NjI0ODkwLDU3MDI3NDc3MiwxMjczMzk0ODY0LC0xNjM1
MjU3OTY4LDM4MjA1NDk5OSwtOTAyNjY1ODUwXX0=
-->