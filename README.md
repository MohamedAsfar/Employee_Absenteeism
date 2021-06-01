# Employee_Absenteeism [Python,SQL,Tableau]


![absence-700x525](https://user-images.githubusercontent.com/68263684/107470144-27786e80-6b28-11eb-840f-ff9b9c0ea44a.jpg)



---

### Table of Contents
You're sections headers will be used to reference location of destination.

- [Overview](#Overview)
- [Discover](#Discover)
- [Preprocessing the data](#Preprocessing-the-data)
- [Exploratory Data Analysis](#Exploratory-Data-Analysis)
- [Machine Learning](#Machine-Learning)
- [SQL Integration](#SQL-Integration)
- [Business Insights in Tableau](#Business-Insights-in-Tableau)

---

## Overview

The total yearly expense lost in productivity due to absenteeism in a workplace is about 24.2 billion (Grace Madlinger, 2018). The elevated risk of being unemployed during the COVID-19 scenario causes depression and illness in employees. As per CareerBuilder's yearly overview, the probability of an employee being absent for more hours has increased by 40% when compared to the last year. The main reason behind this project is to help the organizations predict employee absenteeism so that it can take appropriate measures to prevent a decline in productivity. A dataset is taken from the 365datascience website and a machine learning model is built using classification algorithms, with significant variables like “Reasons for Absenteeism” and “Absenteeism in hours”, to predict whether the employee is excessively absent or not. The best model Logistic Regression is selected based on accuracy (74%) and the process of data manipulation is made smooth by integrating Python with SQL. Further, the predicted outputs are analyzed in Tableau which helps to scrutinize details of the employee in-depth.

---
## DISCOVER
The entire dataset was collected from 365DataScience.com. It consists of twelve columns and seven hundred rows. The following Table 1 explains each variable in detail. 
| Variable  | Data type |
| ------------- | ------------- |
| ID  | Integer  |
| Reason for Absence  | Integer  |
| Date	  | String  |
| Transportation Expense  | Integer  |
| Distance to Work  | Integer |
| Age  | Integer  |
| Daily Work Load Average  | Integer  |
| Body Mass Index | Integer  |
| Education  | Integer  |
| Children  | Integer  |
| Pets  | Integer  |
| Absenteeism Time in Hours  | Integer  |
A new column called “Excessive Absenteeism” will be created from the dataset and employees with excessive absenteeism will be predicted after preprocessing the data. The categorical variables and numerical variables will be checked by preprocessing the data and further a machine-learning model will be created to predict employee absenteeism. 
The transportation expense is a subset of the entire travel expense. It is an addition of Gas cost, transportation cost, meal cost, or any other cost in which an employee can claim for reimbursement. So, this column consists of all the expenses related to travel for each month in dollars. The Distance to work denotes the number of kilometers the employee should travel to reach the office. This is an important feature as it highly influences the decision of the employee to take leave or not. The daily workload average column denotes the amount of work time spent by an employee in a day in minutes. For example 239.554 in the first row denotes that the employee works for an average of four hours per day. The body mass index column denotes whether the employee is overweight, underweight, or normal. There is a common thing between children and pets columns. These columns have categorical data containing numerical values. Whereas Education contains numbers ranging from 1 to 4. 
---
## Preprocessing the data

The target column is "Absenteeism Time in Hours" . If the dataset has 1% or less than one percent of missing data, it can be neglected. But, on the other hand, if there are more missing data, the entities should be replaced by either mean, median, or mode.  
The dataset consists of zero null values for each feature. The first row in the dataset shows that an employee with ID number 11 is absent for four hours in July 2015. The employee ID number is a unique identification number for each person in the workplace. This information will be helpful to track the presence of a particular employee. For a broader picture, it doesn't help for analyzing the problem statement. The Machine Learning model will be not valid if such features are included. So, column “ID” is dropped. 
It is also necessary to group the reasons for absenteeism because having 28 different reasons for the analysis increases the complexity of the machine learning. As a result, the 28 reasons are grouped into four major categories. The resons were grouped as 

- Reason_1 grouped by diseases
- Reason_2 grouped by pregnancy issues
- Reason_3 grouped by poisoining
- Reason_4 grouped by general body issues

Now, lets focus on our target variable "Absenteeism Time in Hours". The meadian value in "Absenteeism Time in Hours" is 3 hours. So, we group them as 

- Moderately absent ( less than or equal to 3 hours -Class value 0)
- Excessively absent (greater than 3 hours - Class value 1)

---

## Exploratory Data Analysis
### Some of the striking features related to Absenteeism

- Months Vs Excessive Absenteeism

![EDA 1](https://user-images.githubusercontent.com/68263684/107476943-1897b900-6b34-11eb-8132-1f1520791d81.png)

- Days Vs Excessive Absenteeism

![EDA 2](https://user-images.githubusercontent.com/68263684/107477034-4a108480-6b34-11eb-9b3f-6f741d64733d.png)

- Distance to work

![EDA 3](https://user-images.githubusercontent.com/68263684/107477083-63193580-6b34-11eb-9861-94f452bc2a38.png)



### Points to note

- The rate Excessive Absenteeism is increses from January to December
- Employees were mostly absent during Mondays and Fridays
- As the Distance to work increase, Employees are prone to be absent

---

## Machine Learning

As the prediction Varible is classified as either Clas 0 or Class 1, Logistic Regression is used to create a Machine Learning model. There were nearly 46% Class 0's amn 54% Class 1's which shows that the dataset is almost balanced. Training and test sets were splitted based on the 80-20 ratio. The model created resulted in an accuracy of 78% which is above baseline. The results from the model are as follows:


| Feature name | 	Coefficient	| Odds_ratio | 
| ------------- | ------------- |------------- |
| Reason_3 | 	3.092102 | 	22.023327 | 
| 	Reason_1 | 	2.771512 | 	15.982778 | 
| Reason_2 | 	0.931688 | 	2.538791 | 
| 	Reason_4 | 	0.809059 | 	2.245794
| 	Transportation Expense | 	0.625055 | 	1.868348
| 	Children | 	0.357535 | 	1.429801
| 	Body Mass Index | 	0.288294 | 	1.334150
| 	Month Value | 	0.007812 | 	1.007843
| 	Age | 	-0.173903 | 	0.840378
| 	Education | 	-0.240816 | 	0.785986
| 	Pets | 	-0.273374 | 	0.760808
| Intercept | 	-1.609575 | 	0.199973

From the above table, it is clear that the Reason_3(poisoining), Reason_1 (disease), Reason_2 (pregnancy issues) are the major factors contributing for excessive absenteeism.

---
## SQL Integration

SQL is intergrated with Jupyter Notebook using the library pymysql. As an analyst, when we deal with huge amount of data, it is always better to use SQL for data manupulation rather than working in the same Jupyter Notebook where the Machine Learning model is created. The databease and a table named "predicted_outputs" is created in MYSQL workbench and the the data predicted from the machine learning model are imported via Jupyter Notebook. The table created contains all the columns including the reasons, probabilities and predicted values. These values are saved as a csv file which will be visualized in Tableau. 

![Screenshot (86)](https://user-images.githubusercontent.com/68263684/107481349-43394000-6b3b-11eb-8471-046611d8237b.png)



## Business Insights in Tableau

[Click here to see the Visualization's in Tableau](https://public.tableau.com/profile/mohamed.asfar.babul.hussain#!/vizhome/Portfolio2_16128568419720/ReasonsVSProbabilty)


---
[Back To The Top](#Employee_Absenteeism)

