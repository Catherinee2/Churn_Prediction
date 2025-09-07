# Power BI End-to-End Customer Churn Analysis Project

This project demonstrates an **end-to-end customer churn analysis**, from data extraction and transformation to visualization in **Power BI**, and predictive modeling using **Machine Learning with Python**.  

The goal is to **identify churn patterns**, understand customer segments at risk, and **predict future churners**, providing actionable insights for business stakeholders.

## 1. Project Overview
Customer churn is a critical issue for businesses, especially those dealing with **recurring customer data**.  
This project provides a **comprehensive solution** for analyzing and predicting customer churn using a **Telecom customer dataset**.  

It covers the entire lifecycle:
- Raw data ingestion into **SQL Server**  
- Building an **interactive Power BI dashboard** for historical analysis  
- Implementing a **Random Forest ML model in Python** to predict future churners  


## 2. Project Goals
The project aims to achieve the following:

- **Analyze Customer Data**: Study customer behavior at various demographic, geographic, account, and service levels.  
- **Study Churn Profile**: Identify key characteristics of churned customers and pinpoint areas for targeted marketing campaigns.  
- **Predict Future Churners**: Develop a robust ML model to foresee which customers are likely to churn.  

**Key Metrics Tracked**:  
- Total Customers  
- Total Churn  
- Churn Rate  
- New Joiners  

## 3. Data Description
The project uses a **Telecom customer churn dataset**.  
Each row represents a **unique customer** identified by `Customer ID`.  

### Columns include:
- **Demographics**: Gender, Age  
- **Geographics**: State  
- **Referrals**: Number of referrals made  
- **Services Subscribed**: Internet, streaming, phone services, online security, device protection.  
- **Payment & Revenue**: Monthly charges, total revenue, payment method, contract type, tenure in months  
- **Customer Status**: `Churned`, `Stayed`, or `Joined`  
- **Churn Details**: Category and reason for churn  

## 4. Methodology
The project follows a **structured approach**: ETL -> Visualization -> Machine Learning.

### ETL Process (SQL Server)
Implemented in **Microsoft SQL Server** with **SQL Server Management Studio (SSMS)**.  

1. **Database Creation**:  
   - `DB_Churn` created to store project data.  

2. **Table Creation & Data Loading**:  
   - **SHG_Churn** staging table created using the Import Flat File Wizard.  
   - **Data Exploration**: Queries run to check distribution, distinct values, and nulls.  
   - **Data Cleaning**: Null values replaced with defaults (e.g. `'None'` for missing deal values).  
   - **Prod_Churn** production table created with cleaned data.  

3. **View Creation**:  
   - `VW_Churn_Data`: Includes only `Churned` and `Stayed` customers (for ML training).  
   - `VW_Join_Data`: Includes only `Joined` customers (for prediction).  

### Power BI Dashboard & Visualization
The **interactive Power BI dashboard** leverages `Prod_Churn` with custom data transformations.  

1. **Data Import & Transformation**  
   - Imported via **Import Mode**.  
   - **Custom Columns**:  
     - `Churn_Status` → 1 for `Churned`, 0 for `Stayed/Joined`.  
     - `Monthly_Charge_Status` → Categorized into `<20`, `20-50`, `50-100`, `>100`.  
   - **Mapping Tables**:  
     - `Mapping_Age_Grp` and `Mapping_Tenor_Grp` to categorize age/tenure.  
   - **Unpivoting Services**: Converted wide service columns into `Service` and `Status`.  

2. **Measures Created (tbl_measures)**  
   - Total Customers = Count(Customer ID)  
   - New Joiners = Count where `Customer Status = Joined`  
   - Total Churn = Sum(`Churn_Status`)  
   - Churn Rate = Total Churn / Total Customers  

3. **Dashboard Pages**  
   - **Summary Page**: Metrics, churn distribution, demographics, services, churn reasons (with tooltip drilldowns).  
   - **Churn Prediction Page**: Profiles of predicted churners, customer grids, and risk factors.  

4. **Formatting & Interactivity**  
   - Consistent color themes, slicers, tooltips, AI narrative visuals, navigation buttons.  

### Machine Learning Churn Prediction (Python)
Developed in **Jupyter Notebook** using **Random Forest Classifier**.  

1. **Algorithm**: Random Forest chosen for interpretability and strong performance.  

2. **Data Preparation**:  
   - Exported `VW_Churn_Data` (training) and `VW_Join_Data` (prediction) into `prediction_data.xlsx`.  
   - Dropped irrelevant columns (`Customer ID`, `Churn Category`, `Churn Reason`).  
   - Encoded categorical variables using **LabelEncoder**.  
   - Split into 80% training / 20% testing.  

3. **Model Training**:  
   - Trained RandomForestClassifier with 100 trees.  

4. **Model Evaluation**:  
   - **Confusion Matrix**:  
     ```text
     [[802   54]
      [117  229]]
     ```  
     - True Negatives (802) → Correctly predicted `Stayed`  
     - False Positives (54) → Wrongly predicted `Churned`  
     - False Negatives (117) → Missed churners  
     - True Positives (229) → Correctly predicted churners  

   - **Accuracy**: 84%  
   - Model performed better for predicting `Stayed` customers (class imbalance observed).  

5. **Prediction on New Data**:  
   - Applied trained model to `VW_Join_Data`.  
   - Final output saved as CSV and loaded into Power BI.  
   - **372 predicted churners** identified.  

---

## 5. Key Metrics & Insights
- Total Customers, Total Churn, Churn Rate, and New Joiners tracked in real-time.  
- **Demographic & Service Insights**: Older females without device protection churn more frequently.  
- **Service Gaps**: Lack of security or protection plans linked with higher churn.  
- **Predictive Power**: ML model proactively flags **372 at-risk customers**, enabling targeted retention campaigns.  

---

## 6. Technologies Used
- **Database**: Microsoft SQL Server  
- **Database Management**: SQL Server Management Studio (SSMS)  
- **Business Intelligence**: Microsoft Power BI Desktop  
- **Programming**: Python 3.0+  
- **Development**: Jupyter Notebook  
- **Python Libraries**:  
  - Pandas (data manipulation)  
  - Scikit-learn (ML: LabelEncoder, train_test_split, RandomForestClassifier, metrics)  
  - NumPy (numerical operations)  
  - Matplotlib, Seaborn (visualizations)  
 


