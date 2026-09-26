# Bank Marketing Classification
# Project Overview
The goal of this project is to build a classification model that can help a bank decide which customers should be prioritized for telephone marketing calls. The model predicts whether a customer is likely to subscribe to a term deposit based on information from previous marketing campaigns.
The model is only meant to help prioritize calls. It is not meant to automatically decide whether a customer can open an account or receive a financial product.
# Dataset
I used the Bank Marketing dataset from the UCI Machine Learning Repository. The full dataset contains 45,211 rows.
Dataset citation:
Moro, S., Rita, P., & Cortez, P. (2014). Bank Marketing [Dataset]. UCI Machine Learning Repository. https://doi.org/10.24432/C5K306
License: CC BY 4.0
# Data Preparation
The target variable is y. A value of yes means the customer subscribed to a term deposit and is treated as the positive class.
I removed the duration column before training the models. This column tells us how long the completed phone call lasted, which would not be known before the bank decides who to call. Using it could make the model look better than it really is because that information would not be available when making a future prediction.
After removing duration, I had 15 input features.
The numeric features were:
age
balance
day
campaign
pdays
previous
The categorical features were:
job
marital
education
default
housing
loan
contact
month
poutcome
I used StandardScaler for the numeric features and OneHotEncoder for the categorical features.
The data was split into 80% training data and 20% testing data. I used a stratified split so that the percentage of subscribers stayed similar in both sets.
About 11.7% of the customers in the dataset subscribed, while about 88.3% did not. Because the classes are unbalanced, accuracy by itself is not enough to judge the models.
# Models
I trained and compared two classification models:
1. Logistic Regression
2. Random Forest
# Model Results
At the default decision threshold, the models produced these results:
Metric / Logistic Regression / Random Forest
Accuracy / 0.8933 / 0.8937
Precision / 0.6632 / 0.6222
Recall / 0.1786 / 0.2335
F1 Score / 0.2815 / 0.3395
False Positives / 96 / 150
False Negatives / 869 / 811
True Positives / 189 / 247
Both models had about 89% accuracy, but the other metrics showed important differences. Random Forest had higher recall and F1 score and found more customers who actually subscribed. Logistic Regression had higher precision and produced fewer false positives.
This shows why I did not choose a model based only on accuracy.
# False Positives and False Negatives
A false positive happens when the model predicts that a customer will subscribe, but the customer does not actually subscribe. For the bank, this means spending time and money on a call that does not result in a subscription. For the customer, this could mean receiving an unwanted marketing call.
A false negative happens when the model predicts that a customer will not subscribe even though the customer actually would subscribe. For the bank, this could mean missing a potential customer. From the customer's point of view, they may not receive a call about a product they could have been interested in.
The importance of these errors depends on what the bank wants from the campaign. Reducing false positives can cut down on unnecessary calls, while reducing false negatives can help the bank find more potential subscribers.
# Decision Threshold
I tested decision thresholds from 0.20 to 0.70.
Lowering the threshold increased recall and allowed the models to find more customers who actually subscribed. The tradeoff was that it also increased false positives, which would cause the bank to make more unproductive calls.
Raising the threshold increased precision and made the call list more selective, but it also caused the models to miss more customers who actually subscribed.
For example, Random Forest at a threshold of 0.50 produced 253 true positives, 157 false positives, and 805 false negatives.
When I lowered the Random Forest threshold to 0.30, it produced 474 true positives, 573 false positives, and 584 false negatives. This found 221 more actual subscribers, but it also created 416 more false positives.
This shows that there is not one perfect threshold. The bank has to decide how important it is to find more potential subscribers compared with reducing unnecessary calls.
# Model Choice
Based on my results, I would use Random Forest for this marketing problem. At the default threshold it had higher recall and a higher F1 score than Logistic Regression, while the accuracy of the two models was almost the same.
I would also consider using a threshold lower than 0.50 if the marketing team wants to find more potential subscribers. A lower threshold improves recall, but the bank would need to accept that the call list would be larger and include more customers who do not subscribe.
The final threshold should depend on how the bank values the cost of an extra marketing call compared with the cost of missing a potential subscriber.
# Fairness and Privacy
Some of the fields in this dataset could create fairness or privacy concerns in a real marketing system.
Age and marital status are examples of personal characteristics that could cause concerns if certain groups receive different marketing treatment. Job and education could also indirectly represent social or economic differences between customers.
The dataset also contains financial information such as balance, housing loans, personal loans, and default history. This information should be handled carefully because it contains private financial information.
I kept these fields for this classroom experiment, but a real bank should review whether each feature is necessary and appropriate before using it. The bank should also test the model for unfair differences between customer groups and follow its privacy and compliance requirements.
This model is only intended to prioritize marketing calls and should not be used to determine whether a customer is eligible for a bank account or financial product.
# Setup
The project was created using Python and Jupyter Notebook.
The main packages used were:
pandas
scikit-learn
matplotlib
The packages can be installed with:
python -m pip install pandas scikit-learn matplotlib ucimlrepo
The main notebook is:
bank_marketing_classification.ipynb
Run the notebook cells from top to bottom to load the dataset, prepare the data, train the models, evaluate the results, and test different decision thresholds.
# AI Assistance
I used ChatGPT as a learning aid while completing this project. I used it to help understand classification concepts, troubleshoot Python errors, organize parts of the project, and better understand metrics such as precision, recall, F1 score, false positives, false negatives, and decision thresholds.
I verified the results by running the code in my own Jupyter Notebook, checking the model outputs and confusion matrices, comparing the calculated metrics, and reviewing the results used in my conclusions.