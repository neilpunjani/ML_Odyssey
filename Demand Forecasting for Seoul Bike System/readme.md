## Problem Introduction

The dataset used in this project, **“Seoul Bike Sharing Demand”**, contains hourly bike rental data from December 2017 to November 2018. The target variable, **“Rented Bike Count”**, represents the number of bikes rented per hour. The dataset includes 13 features, such as time-based attributes (**Date, Hour, Seasons**) and weather-related factors (**Temperature, Humidity, Visibility**).

This project focuses on **demand forecasting** to predict the number of bikes required at different hours using historical environmental, temporal, and cultural data. Accurate forecasting can optimize supply chains by:

**1. Preventing Demand-Supply Mismatch**: Reduces lost revenue from unmet demand and avoids unnecessary maintenance costs due to over-allocation.

**2. Enhancing Operational Efficiency**: Provides structured bike allotment and prevents unnecessary redistribution from warehouses.

**3. Improving Financial Decision Making**: Helps Seoul Bike Service optimize budgets, fleet size, and labor planning.

**4. Increasing Customer Satisfaction**: Ensures low wait times and met demand, improving user experience and loyalty.

<h2>Approach</h2>

### Data Cleaning  
No missing/duplicate values. Removed **“Functioning Day”** (No) & **“Date”**.  

### Data Processing  
Engineered **hour** as cyclical features, one-hot encoded **Seasons**, binary encoded **Holiday**, standardized continuous features.  

### Modeling  
Used **Linear Regression** & **Random Forest**, evaluated with **RMSE, MAE, R²**, extracted feature importances. Ran **Azure AutoML (300 min)** for optimization. Implemented in **Jupyter Notebook in Azure ML**.  

The initial model will be coded in a Jupyter Notebook in Azure Machine Learning environment to ensure smooth integration of datasets and results with Azure Auto ML which will be the last step of your approach. 

**Azure Machine Learning Jupyter Notebook Environment:**

![image](https://github.com/user-attachments/assets/2e2536ea-218b-492a-a3fd-730a14c1696b)

<h2>Some results of Exploratory Data Analysis (All Graphs in PDF)</h2>

**1. Feature Correlation Heatmap:**

![image](https://github.com/user-attachments/assets/48dbfc93-7e6e-47e7-954d-f8626943d790)

This heatmap shows correlations between numerical features ranging from +1 to -1. Most features have low correlation, as indicated by the color intensity, meaning they capture unique information. This helps identify feature relationships and potential multicollinearity for better feature selection.  

**2. Violin Plot to show Demand Density Distirbutions for Different Seasons**

![image](https://github.com/user-attachments/assets/5daf7033-0377-4b52-aedb-6de2a06c7c2b)
  
This violin plot shows bike rental distribution by season. Summer peaks around 800, winter has the lowest, while spring and autumn show higher variability with outliers.  

**3. Distribution of Bikes rented by time of day for different seasons**

![image](https://github.com/user-attachments/assets/0e9c7c51-4b56-439a-9033-aaacbe10e0e5)

Bike rentals peak at **8 AM and 6 PM**, aligning with commute times. Rentals are generally higher in the afternoon/evening than at night or early morning. Notably, peak hours remain consistent across all seasons.  

<h2>Feature Engineering and Final Preparation</h2>

- **Hour Encoding:** Converted 0-23 hour values into cyclical features (sin, cos) to preserve relationships, then removed the original column.  
- **Season Encoding:** Applied one-hot encoding (4 binary columns) for nominal seasons.  
- **Holiday Encoding:** Converted to binary (1 = No Holiday, 0 = Holiday).  
- **Standardization:** Used StandardScaler for numerical features, keeping categorical features unchanged for interpretability.  

[Reference](https://shrmtmt.medium.com/understand-the-capabilities-of-cyclic-encoding-5b68f831387e)  

<h2>Linear Regression and Random Forest Deployment</h2>
Data was split **75% train / 25% validation** post-standardization. Measures chosen for evaluation:  
- **RMSE:** Penalizes larger errors, keeping scale interpretable.  
- **MAE:** Measures average absolute error, useful with RMSE for outliers.  
- **R²:** Evaluates model fit, ranging from 0 to 1 as a benchmark.  


<h3>Linear Regression</h3>

Used as a **baseline model** to detect overfitting and assess non-linear patterns. Its **interpretable coefficients** reveal feature influence on the target variable.  

**Code:**

![image](https://github.com/user-attachments/assets/b02a72b1-fe53-4dda-a054-e9be489eab80)

**Feature Importance Plot**

![image](https://github.com/user-attachments/assets/1a5eaa9c-7f52-4012-a491-3c08fad3508b)

Linear regression performed poorly (RMSE: 436.29, MAE: 324.45, R²: 0.53), failing to capture non-linearity. Dew point temperature and humidity were top predictors, but due to poor performance, feature importance is unreliable. A more advanced model is needed.  

<h3>Random Forest</h3>

Random Forest captures **non-linearity** and **feature interactions** without extra engineering. It **mitigates overfitting** by averaging trees and is **resistant to outliers** through partitioning.  

**Code**

![image](https://github.com/user-attachments/assets/0f6965fc-34f5-40e8-ae5e-c99808b1120d)

**Feature Importance Plot**

![image](https://github.com/user-attachments/assets/80fc1f5c-1358-46be-a030-9a75d4bf2cb5)

Random Forest outperformed linear regression (RMSE: 254.02, MAE: 153.01, R²: 0.84), confirming non-linearity. While RMSE suggests outliers, low MAE indicates strong performance. **Temperature** and **hour_sin** were key predictors, matching EDA insights.  

<h2>In Depth Exploration of Tree - Based and Ensemble Models to enhance performance of algorithm using Azure Auto ML</h2>

**Top Models:**

![image](https://github.com/user-attachments/assets/46f2fd35-eb79-4368-8718-b8b94dfbe840)

**Result on best model on Unseen Test Data:**

![image](https://github.com/user-attachments/assets/47d0bb16-0fd6-41a8-a520-c9b9140df4cc)

**Feature Importance:**

![image](https://github.com/user-attachments/assets/8c901cf6-c253-49ec-b93e-24ce9e7113fb)

AutoML ran for **5 hours (300 min), testing 254 models**. The best model, **Voting Ensemble (LightGBM + XGBoost)**, achieved **R²: 0.88, MAE: 142.08, RMSE: 220.62**, slightly improving over Random Forest. Performance on unseen data was similar, indicating **low overfitting**. **Temperature** and **hour** were the key predictors, while **snowfall had minimal impact**. Gains were incremental, suggesting dataset limitations or missing external features. Future work includes **Independent and combined time-series analysis, hyperparameter tuning, stacking ensembles, and external data integration**.  

