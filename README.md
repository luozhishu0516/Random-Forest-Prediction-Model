📖 Project Overview
This machine learning project predicts cattle movement patterns for the CCIA IT department. Using Random Forest algorithms, we developed two predictive models:

Move-In Date Prediction: Forecasts when cattle will move into new locations based on birthdates

Destined Account Prediction: Predicts which account cattle will be assigned to based on source accounts

🎯 Business Problem
The agricultural industry needs accurate forecasting of cattle movements to optimize resource allocation and operations planning. This solution helps predict:

When cattle will move to new locations (±30 days accuracy)

Which accounts cattle will be assigned to (93%+ accuracy)

📊 Dataset
The dataset contains 27,378 observations with the following features:

Birthdate: Cattle date of birth

Move-in Date: Historical movement dates

Source Account: Origin account

Destined Account: Target account

TAG_START: Unique cattle identifier

🛠️ Methodology
Data Preprocessing
Merged birthdate and move-in datasets

Removed duplicate entries and irrelevant columns

Filtered outliers (dates before 2020)

Handled missing values appropriately

Feature Engineering
Created temporal features to improve model performance:

age_at_movein_days: Days between birth and move-in

dob_month: Month of birth

dob_weekday: Day of week of birth

movein_month: Month of move-in

movein_weekday: Day of week of move-in

EVENT_DATE_numeric: Numerical date representation

Model Selection
Random Forest was chosen because it:

Handles complex feature relationships

Resists overfitting

Provides feature importance metrics

Works well with mixed data types

📈 Results
Move-In Date Prediction
Training set: 21,549 observations

Test set: 5,385 observations

Accuracy: 83.5% within ±30 days margin of error

Destined Account Prediction
Training set: 21,549 observations

Test set: 5,385 observations

Accuracy: 93.47%

Distinct accounts: 5 source accounts, 5 destination accounts
