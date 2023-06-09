# Employee_Attrition_Prediction
Table of Contents
1. [Overview](#overview)
2. [DataSource](#datasource)
3. [Results](#results)
   * [Data Preprocessing](#data-preprocessing)
   * [Data Visualization](#data-visualization)
   * [Machine Learning](#machine-learning)
5. [Summary](#summary)

## Overview 
To verify if the duration of employment depends on employee demografics, this project trys to discover which are the main effects of employees to make their decision on leave or stay in the company. Based on the analysis, build machine learning models to make predictions and calculate the model accuracy.
   - Analyze employee's charactistics who left or stayed in the company.
   - Use machine learning to build models and check model performance on predictions. 

## DataSource
   [Human_Resources.csv](https://github.com/CelineWW/Employee_Attrition_Prediction/blob/main/Human_Resources.csv)
   - This is a clean dataset, including 1470 employees information without unkown or missing value.
   - 35 Columns: 
      - 7 object columns: ***Attrition, BusinessTravel, Department, EducationField, Gender, JobRole, MaritalStatus***
      - 28 number columns: ***Age, DailyRate, DistanceFromHome, Education, EmployeeCount, EmployeeNumber, EnvironmentSatisfaction, HourlyRate, JobInvolvement, JobLevel, JobSatisfaction, MonthlyIncome, MonthlyRate, NumCompaniesWorked, Over18, OverTime, PercentSalaryHike, PerformanceRating, RelationshipSatisfaction, StandardHours, StockOptionLevel, TotalWorkingYears, TrainingTimesLastYear, WorkLifeBalance, YearsAtCompany, YearsInCurrentRole, YearsSinceLastPromotion, YearsWithCurrManager***
   
## Results
### Data Preprocessing
   - Use lambda function to convert boolen columns to numerical data.
      ```
      employee_df['Attrition'] = employee_df['Attrition'].apply(lambda x:1 if x == 'Yes' else 0)
      employee_df['OverTime'] = employee_df['OverTime'].apply(lambda x:1 if x == 'Yes' else 0)
      employee_df['Over18'] = employee_df['Over18'].apply(lambda x:1 if x == 'Y' else 0)
      ```
   - Use histogram to check distributions of each column
     Remove 3 unimportant columns(single unique value) for further analysis:
     - Over18 Y
     - StandardHours 80
     - EmployeeCount 1
     
     ![hist1](https://user-images.githubusercontent.com/105877888/233161655-0e66f5a0-8e7f-43cc-8cf7-4c68e13d49f1.png)
     ![hist2](https://user-images.githubusercontent.com/105877888/233161731-ff102ca9-2cd2-482e-84aa-6355e634abfc.png)

### Data Visualization
   - Seperate dataset by attrition
      ```
      left_df = employee_df[employee_df['Attrition'] == 1]
      stayed_df = employee_df[employee_df['Attrition'] == 0]
      ```
     - :raising_hand_man: Employee those stayed: 1233 person (83.88%)
     - :no_good_man: Employee those left: 237 person (16.12%)
     
   - The relationships between employee demographics from the view of correlation map 
      - Job level is strongly correlated with total working Years
      - Monthly income is strongly correlated with Job level
      - Monthly income is strongly correlated with total working Years
      - Age is stongly correlated with monthly income    
      
      ![heatmap](https://user-images.githubusercontent.com/105877888/233162403-05dc1f8d-c227-4ab1-89f4-e17c731b81d4.png)

   
   - Differences between the demographics of stayed and left employees from the view of countplots and KDE plots 
      - Countplots
         - Single employees tend to leave compared to married and divorced
         - Sales Representitives tend to leave compared to any other job 
         - Less involved employees tend to leave the company 
         - Less experienced (low job level) tend to leave the company 

         ![countplot_age](https://user-images.githubusercontent.com/105877888/233164158-7455b082-928a-4846-8740-d099a629890b.png)
         ![countplot_married](https://user-images.githubusercontent.com/105877888/233164204-74d407f2-a139-4f9a-9463-6600d8427f2f.png)
      
      - KDE plots
         - Employees who live farther than 10 miles from the company tend to leave
         - Employees younger than 30yrs show higher attrition than over 30yrs 
         - Stay in the same team encourage employees to stay
         - Less than 10 total working years tend to leave the company 
          
         ![kde_age](https://user-images.githubusercontent.com/105877888/233166423-55bd2372-c1f7-447f-8be3-60f0aa18651c.png)
         ![kde_distance from home](https://user-images.githubusercontent.com/105877888/233166502-1af586fa-424e-407c-8373-94a957adff2f.png)
         ![kde_years with current manager](https://user-images.githubusercontent.com/105877888/233166559-8e04e6b3-fdb9-4789-b6c2-43ac555551b2.png)
         ![kde_total working years](https://user-images.githubusercontent.com/105877888/233166593-26c64770-438e-49de-8e8e-9720b8115ec4.png)

      - BoxPlot 
         - The following charts show monthly income based on **Gender** and **JobRole**
      
         ![boxplot_gender](https://user-images.githubusercontent.com/105877888/233167669-63c23e23-d2ba-43e3-b580-ea0490aa09c4.png)
         ![boxplot_jobrole](https://user-images.githubusercontent.com/105877888/233167696-fca58782-e239-45d4-8d55-110bf17077b9.png)

### Machine Learning
   - Data Cleaning
      - Categorical columns
      ![cat_df](https://user-images.githubusercontent.com/105877888/233169351-1ef0b4dd-9342-46f2-a7ea-4a8f1a513b5a.png)

      - Numerical columns
      ![num_df](https://user-images.githubusercontent.com/105877888/233169602-065e344c-567c-4ff8-8604-5e21c485adcf.png)

      - OneHotEncoding and merge dataframe
      ![X_all](https://user-images.githubusercontent.com/105877888/233169912-3896fd92-06ad-46b0-9c3a-20f160484707.png)
      
   - Feature scaling 
      - Features: `X = minmaxscaler.fit_transform(X_all)`
      - Target: `y = employee_df['Attrition']`  
   
   - Split data into training and testing dataset 
      -test size: 0.25

   - Building Model
      - Linear: Logistic Regression
         - Accurarcy: 84.78%
         - Confusion matrix heatmap
         
            ![heatmap_logistic regression](https://user-images.githubusercontent.com/105877888/233171945-e5fdf040-298c-41fc-86c3-558b8a85e1a0.png)

         - classification report
         
            ![report_logistic regression](https://user-images.githubusercontent.com/105877888/233172886-1eaf3250-8aa4-411a-bda9-29a065c09adb.png)

      - Decision Tree Classifier
         - Accurarcy: 80.43%
         - Confusion matrix heatmap
         
            ![heatmap_decision tree classifier](https://user-images.githubusercontent.com/105877888/233172027-5cfcc658-a65d-4fe7-8f45-2bf5fadb1ef5.png)

         - classification report
         
            ![report_decision tree](https://user-images.githubusercontent.com/105877888/233172744-0f6446cf-3e04-4cfe-acdf-fc70acb24652.png)

      - Random Forest Classifier
         - Accurarcy: 82.88%
         - Confusion matrix heatmap
         
            ![heatmap_random forest classifier](https://user-images.githubusercontent.com/105877888/233172072-a063bad3-4a0c-4cbf-b05a-fc04bf9273a5.png)

         - classification report
         
            ![report_random forest](https://user-images.githubusercontent.com/105877888/233172588-c68092f5-8d81-4c3e-9222-77d9a2aa7058.png)

      - XGBoost Classifier
         - Accurarcy: 84.78%
         - Confusion matrix heatmap
         
           ![heatmap_xgboost classifier](https://user-images.githubusercontent.com/105877888/233172111-df0551a7-d187-4e79-821b-5780aee36297.png)

         - classification report
         
            ![report_xgboost](https://user-images.githubusercontent.com/105877888/233172553-5249f016-f15d-4974-9fd1-6a9dc6b9ccaa.png)

## Summary
  - Who tends to leave the company? Single(maritial status), sales representatives (job role), Less involved, less working experienced, whose house far away from the company, younger than 30 years old. Typically, employees at their earlier career stage tend to leave the company compare to those more experiened and need to support family.
  - Logistic regression and XGBoost classifier model got the highest accuracy score(84.78%). 
  - This is an imblanced dataset. F1-score better reflects model performance. Random forest classifier got the lowest f1-score(0.16) on predicting left employees. Logistic regression and decision tree are 0.42, surpass other two models. Overall, logistic regression is best predict the employee attrition.
  - Since the dataset is imbalanced. Resampling such as oversampling or undersampling is suggested to retrain the models.
