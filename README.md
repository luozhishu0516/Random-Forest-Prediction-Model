1.1	 Introduction
In this project, we aim to build a machine learning model to predict the move-in date and the destined account for cattle based on their birthdate and source account. The data we use is provided in two CSV files: one containing birthday information for each cattle and another containing move-in details. By combining and processing these datasets, we will use a Random Forest model, a powerful machine learning algorithm, to make these predictions. The goal is to predict the following:
1.	Move-in Date: Predicting when each cattle will move into its new location.
2.	Destined Account: Determining the account that the cattle is assigned to based on its birthday and source account.
The dataset provided contains 27,378 observations (records), and we will apply a training/test split (80/20) to evaluate the model's performance.







1.2	 Problem Overview
The move-in data consists of records with tags (unique identifiers for the cattle), along with their move-in dates. Each cattle also has a birthday recorded in a separate dataset. The challenge is to use the birthdate and source account information to predict the move-in date and the destined account.
To address this, we will:
•	Preprocess the data by cleaning and merging the two datasets.
•	Perform feature engineering to generate new attributes that may improve the model's performance.
•	Use Random Forest, a robust ensemble learning algorithm, to build the prediction model.







1.3	 Machine Learning Method: Random Forest
Random Forest is a popular machine learning algorithm that creates multiple decision trees and combines their outputs to improve accuracy and reduce overfitting. Random Forest works well with datasets that contain a mix of numerical and categorical data, making it a good choice for this project.
1.3.1	Why Random Forest?
•	Non-linearity: Random Forest can handle complex relationships between features, which might not be easy to capture with traditional linear models.
•	Robustness: It is less prone to overfitting compared to other models like decision trees, especially with a large number of observations.
•	Feature Importance: Random Forest can automatically assess the importance of each feature, which can help in understanding the underlying data.
 <img width="788" height="479" alt="image" src="https://github.com/user-attachments/assets/824d41c7-8dc0-4562-b55a-8792e75de5e2" />





1.4	 Dataset Overview
The dataset consists of the following features:
•	Birthdate: The birthdate of each cattle.
•	Move-in Date: The date when the cattle moves into its new location.
•	Source Account: The account from which the cattle originated.
•	Destined Account: The account to which the cattle is assigned.
•	TAG_START: Unique identifier for each cattle.
The goal is to predict:
1.	Move-in Date (a date value) based on the birthdate and source account.
2.	Destined Account (categorical value) based on the birthdate and source account.
The dataset contains 27,378 observations, and we will split the data into 80% training data and 20% test data for model evaluation.


1.5	 Methodology
The data preprocessing steps involve:
•	Merging the birthday and move-in datasets by the TAG_START column (unique identifier).
•	Cleaning the data by removing duplicates and irrelevant columns.
•	Feature Engineering: Creating new columns such as the cattle’s age at move-in, the month and weekday of the birthdate and move-in date, etc.
•	Model Training: Using the Random Forest algorithm to train a predictive model based on the processed data.
The following sections will explain each of the steps in detail, from data preparation and cleaning to building and evaluating the model.





1.6	 Explanation of the Code (Move-In Date)
This section builds a model to predict the EVENT_DATE based on the DOB using a Random Forest classifier.
1.6.1	 Loading Libraries
To begin, we load the necessary libraries that will assist with data manipulation, machine learning, and model evaluation.
 <img width="975" height="216" alt="image" src="https://github.com/user-attachments/assets/bdad197c-2111-4b7a-8709-45b2a3b061b7" />

1.6.2	Reading the Dataset
The dataset consists of two CSV files: Birthdate and MoveIn. We read them into R using the read.csv() function.
 <img width="975" height="172" alt="image" src="https://github.com/user-attachments/assets/5061a790-4695-490e-b7e7-4271e745a2fc" />


1.6.3	 Date Conversion
Since the original data includes timestamps along with the dates, we standardize the date format to yyyy-mm-dd to remove the time component.
 <img width="975" height="161" alt="image" src="https://github.com/user-attachments/assets/4d5c9b0e-a400-4908-b03b-efbe752ff1d0" />

1.6.4	 Removing Duplicate Records
To ensure the integrity of the data, we remove any duplicate TAG_START entries in both datasets.
 
<img width="975" height="159" alt="image" src="https://github.com/user-attachments/assets/6a6b6a57-4941-4ba7-9636-68fdecbd598f" />



1.6.5	Merging the Datasets
We then merge the birthday data with the move-in data on the TAG_START field to ensure each cattle has both their birthdate and move-in details in the same dataset.
 <img width="975" height="175" alt="image" src="https://github.com/user-attachments/assets/6a2c19d3-e661-4a8d-a4a5-7ac6f33d8726" />

1.6.6	Data Cleaning
To begin with, we load and merge the birthday and move-in datasets using a common identifier. One of the columns, SOURCE_PREMISES, contains mostly missing values and thus is removed from the dataset to reduce noise and avoid misleading the model.
 <img width="975" height="181" alt="image" src="https://github.com/user-attachments/assets/47b2fbeb-05d5-4812-8b5a-14498ea4d7c3" />

1.6.7	Exploratory Data Analysis (EDA)
We visualize the distribution of the DOB (Date of Birth) column to better understand the spread of birth dates in our dataset.
 
 
<img width="875" height="164" alt="image" src="https://github.com/user-attachments/assets/643bce60-b5a9-4dd9-9ae2-f94d5106f1ff" />
<img width="975" height="602" alt="image" src="https://github.com/user-attachments/assets/49b81a22-7e6a-4d8b-adfc-b75504b054f8" />

Upon inspection, some dates before the year 2020 are considered outliers and are filtered out to improve the reliability of our predictions.
 <img width="975" height="309" alt="image" src="https://github.com/user-attachments/assets/a00a4a9c-8644-442a-a6f4-d720d939b8dc" />

After:
 <img width="975" height="602" alt="image" src="https://github.com/user-attachments/assets/86fd008b-c985-4a57-b0a0-0f2397cef58a" />







1.6.8	Feature Engineering
To enhance the model's performance, we generate new features that can capture temporal patterns and relationships:
•	age_at_movein_days: Number of days between birth date and move-in date.
•	dob_month: Month of birth.
•	dob_weekday: Day of the week of birth.
•	movein_month: Month of move-in.
•	movein_weekday: Day of the week of move-in.
<img width="975" height="330" alt="image" src="https://github.com/user-attachments/assets/8649267c-3fdc-402b-94d9-357db6a77225" />

 We also convert the EVENT_DATE column to a numeric format by calculating the number of days since a fixed reference date (e.g., Jan 1, 2000), which simplifies prediction as a regression task.

Reminder: Apply Same Feature Engineering to New Data
Before using your trained model to make predictions on new data, it's essential to repeat the same feature engineering steps you applied during training.
Feature Engineering Steps to Repeat:
•	age_at_movein_days: Difference between EVENT_DATE and DOB
•	dob_month: Extracted month from date of birth
•	dob_weekday: Day of the week for date of birth
•	movein_month: Extracted month from move-in date
•	movein_weekday: Day of the week for move-in date
•	EVENT_DATE_numeric: Number of days since a reference date (e.g., 2000-01-01)
Also, ensure categorical features (like weekdays or months) are converted to factors, just like during training.
<img width="834" height="349" alt="image" src="https://github.com/user-attachments/assets/180bf82f-885b-4875-89e2-7772f504dc46" />

 
1.6.9	Model Training (Random Forest)
We split the data into training (80%) and testing (20%) sets using createDataPartition from the caret package.
 Then, we train a Random Forest model to predict the numeric representation of the move-in date using the engineered features.
 <img width="975" height="244" alt="image" src="https://github.com/user-attachments/assets/73657492-d45f-47d4-9163-af9e5fbb56b8" />

The output rf_model is the trained Random Forest Model for Move-In Date
<img width="975" height="181" alt="image" src="https://github.com/user-attachments/assets/355bcabd-8865-4507-b170-41d4f6f88984" />

1.7	Explanation of the Code (DEST_ACCOUNT)
This section builds a model to predict the DEST_ACCOUNT based on the SOURCE_ACCOUNT using a Random Forest classifier.

1.7.1	Loading Libraries
To begin, we load the necessary libraries that will assist with data manipulation, machine learning, and model evaluation.

 
1.7.2	Reading the Dataset
The dataset consists of two CSV files: Birthdate and MoveIn. We read them into R using the read.csv() function.

 


1.7.3	Drop Unnecessary Columns
The DOB and SOURCE_PREMISES columns are removed since DOB isn’t used here, and SOURCE_PREMISES contains mostly missing values.
 
1.7.4	Select Relevant Columns in Move-in Data

Only TAG_START and DEST_ACCOUNT are retained to simplify the dataset and focus on prediction.
 
1.7.5	Merge Datasets and Check for Missing Values

The datasets are merged using the common key TAG_START, and missing values are checked to ensure data quality.
 



1.7.6	Convert Variables to Factors

SOURCE_ACCOUNT and DEST_ACCOUNT are converted to factors so they are treated as categorical variables during model training.
 
1.7.7	Split Data into Training and Testing Sets

The data is split 80% for training and 20% for testing. Setting the seed ensures the split is reproducible.
 
1.7.8	Train the Random Forest Model

A Random Forest classifier is trained to predict DEST_ACCOUNT using SOURCE_ACCOUNT. Feature importance is tracked, and 100 trees are used in the ensemble.
 








1.8	Result

Predicting Move-in Date
The algorithm was trained on 21,549 observations derived from date of birth (DOB) data in the Birthdate dataset to predict EVENT_DATE in the MoveIn dataset. A Random Forest model was employed for this task. The prediction accuracy was evaluated with a margin of error of ±30 days. When tested on a separate set of 5,385 observations, the model achieved an accuracy of 83.5% within the specified error range.

 Predicting Move-in Premise
The algorithm was trained on 21,549 observations using the SOURCE_ACCOUNT data from the Birthdate dataset to predict DEST_ACCOUNT in the MoveIn dataset. A Random Forest model was utilized for this task. The training data included 5 distinct SOURCE_ACCOUNT values and 5 distinct DEST_ACCOUNT values. When evaluated on a separate set of 5,385 observations, the model achieved a prediction accuracy of approximately 93.47%.
