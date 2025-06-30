                                             
                                            
                                             
                                             
### Project Overview

This project focuses on leveraging operational data to drive strategic business decisions for the bank. The primary objective is to develop a predictive model that estimates the likelihood of a client subscribing to a term deposit, based on a range of client attributes and past campaign interactions.
These combined insights will support the marketing team in designing campaigns tailored to client behaviour and preferences, ultimately improving targeting efforts and increasing term deposit subscriptions.


### Exploratory Data Analysis
##### Imbalanced Target Variable:

- The target variable 'y' is imbalanced, with a significantly larger number of clients not subscribing (no) than those who did subscribe (yes).

  

##### Highly Correlated Independent Features:

- emp.var.rate, euribor3m, and nr.employed showed very high correlations (> 0.9),  which might lead to multicollinearity issues when fitting a logistic regression model.  Dropped emp.var.rate and nr.employed.


##### Duplicate Records:

- Duplicates were dropped.
  

##### Dropped duration variable (to avoid data leakage):

- If we are focusing on targeted marketing, then calls to clients are made after predictions are made, thereby highly predictive of subscription.


##### Feature Engineering

- Grouped months into quarterly periods (Q1–Q4)
- Binned the ages into categorical age groups (<30, 30-50, 51-70, 70+)



### Model Insights: Logistic Regression
##### Pipeline Design:

- Implemented a data preprocessing and model training pipeline incorporating feature scaling and one-hot encoding techniques.
  
- Applied class_weight='balanced' to address class imbalance and enhance model performance.

- Utilised K-fold cross to assess how well your model generalises to unseen data


##### Important Features for Predictive Power:

![feature_importance](https://github.com/user-attachments/assets/3bfd6b99-3bc2-4429-9477-4b022a289938)


- Positive coefficients: indicate features associated with a higher likelihood of subscription.

- Negative coefficients indicate features associated with non-subscription.



### Model Evaluation  Metrics Summary
##### Initial model evaluation:

- The baseline logistic regression model was trained on preprocessed features with standard scaling and one-hot encoding. Initial model evaluation yielded the following results on the test set (before threshold tuning):

- Accuracy: 0.81
- Precision: 0.33
- Recall: 0.68
- F1 Score: 0.44
  
- While the overall accuracy appears high, the model struggles with correctly identifying the minority class (subscribe = 1), leading to low precision and moderate recall. 

- Confusion Matrix

![confusion_matrix](https://github.com/user-attachments/assets/0964509c-e387-4515-b888-11bb6ae87fe1)

  __A visualization showing the true vs. predicted classifications._

- True Negatives (top-left): Model correctly said “No, not subscribe”

- False Positives (top-right): The model incorrectly indicated 'yes' when it should have indicated 'no'. The Marketing team could be chasing the wrong clients.

- False Negatives (bottom-left): The Model missed actual subscriptions.

- True Positives (bottom-right): Model correctly predicted 786 actual subscriptions.

  

### Threshold Tuning:

- Tuned the classification threshold to improve the F1 score. The new custom threshold is 60%

- Precision and recall decreased compared to the default threshold. F1 score improved slightly.


##### Final Model Evaluation:

- Precision: 0.43
- Recall : 0.54
- F1 Score: 0.48

![pr_roc_curve](https://github.com/user-attachments/assets/aa419ab1-e668-4cdc-a2e2-dec3cf2bfc80)

_ROC curve shows how well the model distinguishes between classes._
_This PR curve illustrates the trade-off between precision and recall across different thresholds._


##### Reasons for Metric Selection:

- Recall becomes crucial if the goal is to catch as many true subscribers as possible.
  
- Precision matters if the cost of wrongly targeting uninterested clients is high..

- F1 Score gives you a balance between the Recall and Precision. It’s useful when both false positives and false negatives carry weight, and you need a compromise



### Model Improvement:

- Apply advanced sampling, such as SMOTE, to balance the dataset during training and improve recall without heavily sacrificing precision.

- Utilise Ensemble Models: Train Random Forest or XGBoost and compare with the initial model.

- Hyperparameter Tuning: Use GridSearchCV or RandomizedSearchCV to optimise logistic regression parameters.
  


### Common Characteristics of Clients Who Subscribed

- Clients have had a previous successful outcome with the bank.
- As the time since the last campaign contact increases, the likelihood of client subscription tends to decrease.
- Contacted in the 1st quarter of the year.
- Retired clients.
- Clients prefer being contacted through cellular.



### Actionable Recommendations for the Marketing Team

- Focus on clients who had previous success.

- Prioritise Cellular Contact: Cellular contact methods show a higher association with success than telephone.

- Campaign Timing: Target campaigns in the 1st quarter.

- Economic Indicator: When euribor3m is high, clients might anticipate higher deposit interest rates from the bank, making subscriptions more appealing.

- The bank has a nearly normal distribution of client ages can be a great asset for a marketing team. Since age often correlates with financial goals, the marketing team can more confidently align age ranges with product needs. For instance:

<30: Student loans, savings plans.

 30-50: Mortgages, investment options.

60+: Retirement accounts, wealth management.







