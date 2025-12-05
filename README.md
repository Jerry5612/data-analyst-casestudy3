# Data Analytics Capstone: TelcoNow Case Study
## Introduction
In this case study, I will perform many real-world tasks of a junior data analyst at a fictional company, Global Mart. In order to answer the key business questions, I will follow the steps of the data analysis process: Ask, Prepare, Process, Analyse, Share, and Act.
### Quick links:
Data Source: [kaggle_dataset](https://www.kaggle.com/code/farazrahman/telco-customer-churn-logisticregression/input) [accessed on 27/11/25]

## Background
### TelcoNow
TelcoNow is a mid-sized, nation-wide telecommunications provider offering mobile phone, fixed-line telephone, and internet services via subscription contracts. The company relies on consistent monthly recurring revenue (MRR) generated through its diverse subscription plans and value-added product bundles. The platform ensures reliable network performance and contractual flexibility, maintaining its appeal in a highly competitive market.

Until now, TelcoNow's marketing strategy has focused mainly on new customer acquisition through large promotional discounts. The customer base is composed of diverse consumer types, but the most valuable ones are those with long tenure and subscribed to multiple bundled services.

The highly engaged segment, defined by those on long-term contracts (one or two years) and multiple premium services (Fiber Optic Internet, streaming TV), maintains the highest Customer Lifetime Value (CLTV). This segment benefits from consistent service and bundled savings. The less engaged, often lower-value segment includes customers on month-to-month contracts and single, basic services.

TelcoNow's financial analysts have concluded that customer churn (cancellation) poses the largest threat to the platform's future profitability, as the cost of acquiring a new customer significantly outweighs the cost of retaining an existing one. Moreno, the marketing director, believes that proactively identifying and retaining high-risk, high-value customers is essential to minimising costs and maximising future profits.

Rather than distributing expensive, generic retention offers (like blanket discounts) to potentially unsatisfied customers, Moreno believes that utilising a predictive model to identify the highest-risk customers is the most efficient strategy, since marketing budget is limited.

Moreno has set a clear goal: Develop an analytical model that predicts which high-value customers are most likely to churn in the next quarter. The data science team, therefore, needs to understand which customer behaviours and service features are the strongest statistical predictors of churn, how accurate the model can be, and its risk scores can be used to create targeted, personalised retention offers. Moreno and her team are interested in analysing TelcoNow's historical customer and service data to build this predictive framework.

## Ask
### Scenario
I assume the role of Junior Data Scientist working in the marketing analytics team at TelcoNow. The marketing director believes the company’s future financial stability depends on minimising customer churn and maximising retention among high-value subscribers. Therefore, my team needs to understand which behaviour and service features would likely cause customers to cancel their services. These insights would enable my team to design a new, targeted retention strategy. However, TelcoNow executives must approve our recommendations, hence the strategy must be backed up with a compelling predictive model and professional analysis using R as well as presented in professional data visualisations.

### Business Task
Develop a statistically robust predictive model in R that identifies and segments high-risk customers to optimise the marketing retention budget and minimise the costly reliance on new customer acquisition for TelcoNow.

### Analysis Questions
Three questions will guide the predictive modeling program:
1. Feature Analysis: How do customers who churn differ from those who remain in terms of their service features (eg. contract length, internet service, payment method) and usage patterns (eg. tenure, monthly charges)?
2. Predictive Modeling: What is the accuracy and reliability of the R model in predicting future churn, and what risk score threshold should be established to identify the optimal target audience for intervention?
3. Strategic Action: Based on the model's risk scores and feature analysis, what personalised retention offers should TelcoNow use through its digital channels to convince the highest-risk customers to remain as subscribers?

## Prepare
### Data Source
I will use Telco Customer Churn dataset, a publicly available, historical record of customer accounts which can be downloaded from kaggle_dataset. This dataset provides a detailed snapshot of customer behaviours and service subscriptions, which is ideal for training a predictive model in R. The dataset is structured at the customer level, with each row representing a unique subscriber and containing their demographics, service usage (eg. tenure, internet type), as well as the crucial binary status of whether they churned or not. However, as with most public and sensitive customer datasets, privacy constraints prohibit the use of personally identifiable information (PII) such as names, addresses, or payment account details. This means that I will not be able to link churn predictions to external demographic databases. However, the available features (like tenure, MonthlyCharges, and Contract) are sufficient for building a high-performing statistical model.

### Data Organization 
The analysis will be conducted using a single Microsoft Excel Worksheet (xls) file with naming WA_Fn-UseC_-Telco-Customer-Churn.csv.xls and it contains the historical records for 7,043 unique customers of TelcoNow, detailing their subscription status at the end of the analysis period. The dataset is organised by Customer ID and includes 20 other attributes over three main categories: Demographics (eg. Gender, SeniorCitizen), Service Status (eg. PhoneService, InternetService, Contract, MonthlyCharges, TotalCharges), and the crucial target variable: Churn (Yes/No).

## Process
The primary tool for analysis and modeling will be the R programming language, utilising the RStudio environment.

Reason:

While the TelcoNow dataset is small enough to be handled by spreadsheet software like Microsoft Excel, R is essential because the project requires advanced statistical and machine learning techniques that are impossible to perform in spreadsheets. These techniques include:
1. Feature Engineering: Handling categorical variables and converting them for modeling.
2. Binary Classification: Building and training complex predictive algorithms (like Logistic Regression or Random Forests).
3. Model Validation: Calculating and evaluating performance metrics crucial for predictive analysis, such as Precision, Recall, and Accuracy, to ensure the model is reliable for strategic decision-making. 

The project will use specialised R packages (such as tidyverse for cleaning and manipulation, and caret for modeling) to achieve the defined business task of accurately predicting customer churn.
1. I began by installing and loading all essential packages for this project. 

![image](https://github.com/Jerry5612/data-analyst-casestudy3/blob/Jerry5612-patch-3/image6.png)

2. Then, I loaded the data into RStudio.

![image](https://github.com/Jerry5612/data-analyst-casestudy3/blob/Jerry5612-patch-3/image10.png)

### Data Exploration
Before cleaning the data, I am familiarising myself with the data to find the inconsistencies.

1. The picture below shows the structure and data types of the columns from the data that is loaded into RStudio.

![image](https://github.com/Jerry5612/data-analyst-casestudy3/blob/Jerry5612-patch-3/image3.png)

![image](https://github.com/Jerry5612/data-analyst-casestudy3/blob/Jerry5612-patch-3/image14.png)

2. The picture below shows the summary statistics of the dataset (eg. Min, Max, Mean, NA counts).

 ![image](https://github.com/Jerry5612/data-analyst-casestudy3/blob/Jerry5612-patch-3/image8.png)

Summary statistics is checked because it provides a quick numerical profile of the dataset for me to assess the central tendency, spread, and range of the variables. For continuous features like tenure or MonthlyCharges, reviewing the minimum, maximum, mean, and quartiles is crucial for identifying outliers or data entry errors, such as values that fall outside a logical range, which could severely skew my final predictive model.

Furthermore, summary statistics serve as a data validation tool, confirming that R has interpreted the data types correctly. When examining the frequency counts of categorical variables (like Churn), it directly exposed class imbalance (eg. few churned customers). This proactive identification guided the adjustments needed in the predictive classification model to prevent bias toward the majority class during training and evaluation.

### Data Cleaning
1. Handling Missing Values in TotalCharges

Initial data inspection showed that the TotalCharges column was incorrectly interpreted as a text-based (character) variable due to the presence of blank entries. This was corrected by coercing the data to a numeric type by imputing the resulting missing values (NAs) with 0. This change was made because the customers with missing charges were confirmed to have zero tenure, indicating they were new subscribers not yet billed, and thus a charge of zero is the most logical imputation.

![image](https://github.com/Jerry5612/data-analyst-casestudy3/blob/Jerry5612-patch-3/image9.png)

2. Feature Refinement and Transformation

Several variables were adjusted to ensure they were correctly formatted for the predictive classification model.
A. Target Variable Transformation (Churn): The target variable was converted from its text format ("Yes", "No") into a numeric factor (1, 0) respectively. This binary numerical representation is mandatory for training classification algorithms.
B. Categorical Simplification: In several service columns (eg. OnlineSecurity, TechSupport), the category "No internet service" was functionally redundant with "No". These were standardised to a single category, "No" for simpler analysis and cleaner modeling.
C. Variable Type Correction: The SeniorCitizen column was corrected from an integer to a factor to ensure the model treats it as a discrete category rather than a continuous numeric variable.

![image](https://github.com/Jerry5612/data-analyst-casestudy3/blob/Jerry5612-patch-3/image17.png)

3. Structural Cleaning
The customerID column, which serves only as a unique identifier, was removed from the dataset. It has no predictive value and can cause computational noise during the model training process. The final, cleaned dataset is saved as telconow_churn_data_cleaned.

![image](https://github.com/Jerry5612/data-analyst-casestudy3/blob/Jerry5612-patch-3/image4.png)

## Analyse and Share
### 1. Feature Analysis: Key Drivers of Customer Churn
The first analysis question to answer is: How do customers who churn differ from those who remain in terms of their service features (eg. contract length, internet service, payment method) and usage patterns (eg. tenure, monthly charges)?

Exploratory Data Analysis (EDA) was performed to understand the differences between customers who churned (Churn=1) and those who remained (Churn=0). This analysis identified the most significant service features and usage patterns linked to customer cancellation.

### A. Usage Patterns (Continuous Variables)

![image](https://github.com/Jerry5612/data-analyst-casestudy3/blob/Jerry5612-patch-3/image16.png)

Tenure: The analysis revealed a strong inverse correlation between tenure and churn risk. Customers who churned had significantly shorter tenures (median tenure was much lower at 10 months) compared to retained customers. This indicates that churn risk is highest within the first year of a customer's contract.

![image](https://github.com/Jerry5612/data-analyst-casestudy3/blob/Jerry5612-patch-3/image5.png)

Monthly Charges: Churners displayed a higher tendency to be in the premium pricing tier, showing a pronounced peak in monthly charges (at around the $70−$100 range) compared to non-churners. This suggests that customers that pay high prices have high expectations on service standards, leading them to be extremely sensitive to service deficiencies and churn.

### B. Service Features (Categorical Variables)

![image](https://github.com/Jerry5612/data-analyst-casestudy3/blob/Jerry5612-patch-3/image1.png)

Contract Type: Customers on a Month-to-month contract exhibited the highest churn rate of 43% than those on one or two-year contracts, confirming that commitment duration is the primary stabiliser for customer retention.

![image](https://github.com/Jerry5612/data-analyst-casestudy3/blob/Jerry5612-patch-3/image12.png)

Internet Service: Customers subscribed to Fiber Optic internet service showed the highest churn rate of 42% than those using DSL or no internet service. This points to potential reliability issues specific to the high-speed fiber network.

![image](https://github.com/Jerry5612/data-analyst-casestudy3/blob/Jerry5612-patch-3/image7.png)

Payment Method: The analysis indicates that Electronic check payment method was strongly associated with the highest customer churn rate of 45%. This suggests that customers utilising this payment method may be less prepared for a longer-term commitment, or that user experience design for electronic payments is unintuitive.

### 2. Predictive Modeling: Accuracy and Threshold Optimization
The second analysis question to answer is: What is the accuracy and reliability of the R model in predicting future churn, and what risk score threshold should be established to identify the optimal target audience for intervention?

A Logistic Regression model was developed to quantify the relationship between customer features and churn risk, providing a probabilistic risk score for intervention.

### A. Model Training and Interpretation
The data was partitioned into a 70% Training Set and a 30% Testing Set to ensure the model's accuracy was evaluated on unseen data. 
![image](https://github.com/Jerry5612/data-analyst-casestudy3/blob/Jerry5612-patch-3/image11.png)

Examination of the model's coefficients confirmed the EDA findings and established the statistical hierarchy of churn drivers:

#### Strongest Positive Predictors (Features that Increase Churn Risk):
1. Fiber Optic Internet showed the highest isolated risk, with a coefficient (β^​) of 2.456, indicating that this service is the single largest feature-based destabiliser in the model (relative to the baseline).
2. Electronic Check payment method is also a significant risk factor (β^​=0.365), confirming its link to transactional instability.
3. Month-to-month contracts are represented in the model's high baseline risk (Intercept), statistically confirming their role as the primary contract-related risk factor.

#### Strongest Negative Predictors (Features that Reduce Churn Risk):
1. One-year contracts provided the strongest contractual stabilisation (β^​=−7.572), followed by Two-year contracts (β^​=−1.416).
2. Tenure is a highly significant continuous stabiliser (β^​=−5.720), demonstrating that the longer a customer stays, the lower their risk of churning is.
3. Not having Internet Service (a landline-only customer) is a strong stabiliser (β^​=−2.506).

#### B. Model Performance and Threshold Selection 

![image](https://github.com/Jerry5612/data-analyst-casestudy3/blob/Jerry5612-patch-3/image15.png)

Using a standard prediction threshold of 0.50, the model achieved a baseline Accuracy of 0.8021. However, for a retention strategy, Sensitivity (correctly identifying actual churners) must be prioritised over general accuracy.

![image](https://github.com/Jerry5612/data-analyst-casestudy3/blob/Jerry5612-patch-3/image13.png)

The Receiver Operating Characteristic (ROC) Curve was plotted, yielding an Area Under the Curve (AUC) of 0.8465. This AUC value indicates the model has strong discriminatory power in separating churners from non-churners.

![image](https://github.com/Jerry5612/data-analyst-casestudy3/blob/Jerry5612-patch-3/image2.png)

To optimise the marketing budget and maximise the retention of high-value customers, a strategic intervention threshold needs to be identified. Thus, the strategic goal is set to achieve a minimum 70% Sensitivity.
1. Action Taken: The final strategic intervention threshold is set at 0.2401.
2. Resulting Performance: At this threshold, the model delivers a Sensitivity (TPR) of 82.32% and a Specificity (TNR) of 70.81%.

This low threshold ensures high Sensitivity, successfully capturing a large proportion of true high-risk customers, ensuring the efficient allocation of limited marketing expenditure by focusing resources where the retention benefit is highest.

### 3. Strategic Action: Personalised Retention Offers 
The final analysis question to answer is: Based on the model's risk scores and feature analysis, what personalised retention offers should TelcoNow use through its digital channels to convince the highest-risk customers to remain as subscribers?

### A. Target Segment Identification
Based on the goal of maximising customer retention rates, a strategic intervention threshold of 0.2401 was selected. This low threshold ensures the model achieves optimal Sensitivity (capturing a high percentage of true churners).
1. Target Size: Applying this threshold to the test data identifies 914 customers who are classified as "High Risk." These customers represent the target audience for the limited marketing budget.
2. The feature analysis is confirmed by the Logistic Regression model's coefficient estimates (β^​). These values quantify the impact of each variable on the log-odds of churn. The table below presents the top positive and negative predictors that directly inform the strategic segmentation. A positive coefficient indicates an increased likelihood of churn, while a negative coefficient indicates a stabilising effect.

### Key Predictive Findings: Logistic Regression Model Coefficients
#### Churn Drivers (Risk Factors)

The strongest churn-inducing variables are related to service type and contract length. The base risk for churn is associated with the Month-to-month Contract (β=1.818), which serves as the reference risk when other categorical variables are at their base level. The single most destabilizing factor is the Internet Service Fiber optic coefficient (β=2.456), which represents the highest service-related risk of churn compared to the reference (DSL or no internet). Beyond core contracts and services, two transactional factors statistically increase churn risk: using Payment Method Electronic check (β=0.364) and Paperless Billing Yes (β=0.294).

#### Churn Stabilizers (Retention Factors)

Variables that strongly reduce the log-odds of churn are associated with commitment and lack of service. The strongest stabilizer overall is Internet Service No (β=−2.506), indicating that not having internet service is the most significant single factor reducing the risk of churn when compared to the reference internet service. Another highly impactful stabilizer is the long-term commitment provided by the Contract Two year term (β=−1.416), which provides a large single factor reducing churn risk. Finally, tenure acts as a continuous stabilizer (β=−0.057); every additional month a customer stays with the company incrementally reduces their log-odds of churning. 

### B. Synthesis and Offer Recommendation
By analysing the specific characteristics of the high-risk segment, personalised offers can be formulated to directly address the key drivers of their predicted churn. The recommendations below are synthesised from the model's highest positive coefficients and the significant differences found in the Feature Analysis (EDA).

#### High-Risk, Short Tenure, and Contract Commitment

Customers exhibiting short tenure and a month-to-month contract represent a primary risk due to their lack of commitment. The recommended retention offer is a Discounted Upgrade to a 1-Year Contract (e.g., 15% off for 3 months, or a free service for 1 month). This approach is strategically sound because it directly addresses the highest risk factor by stabilizing the customer relationship and locking them into the platform for a longer duration.

#### Fiber Optic Internet and Support Gaps

The second key risk segment involves customers with Fiber Optic Internet who lack supplementary support services, indicating potential service dissatisfaction. The recommendation is to Offer 6 Months of Complimentary Technical Support and Online Security. The rationale is that this addresses their lack of perceived value for a premium subscription and service dissatisfaction, boosting service stability without necessitating a reduction in their base subscription prices.

#### Electronic Check Payment Method

The final high-risk segment uses the Electronic Check Payment Method, which the model statistically links to a higher risk of churn. The personalized offer should Promote an Automatic Billing Discount or offer a switch to bank transfer with a one-time credit (e.g., $10). This strategy incentives a change to a more stable payment mechanism, directly mitigating the transactional risk associated with this specific payment method and improving overall commitment.

## Act
The final phase of the project translates the predictive model's findings into concrete, actionable steps for the TelcoNow marketing and retention teams.

### 1. Project Conclusion and Business Impact
The analysis successfully developed a statistically robust Logistic Regression model that accurately predicts customer churn risk. The model achieved an Area Under the Curve (AUC) of 0.8465 and, using the optimised threshold of 0.2401, yields a high-impact targeting strategy.

The primary threats to TelcoNow's financial stability are confirmed to be:
1. Lack of Commitment: Customers on Month-to-month contracts are the single highest risk group.
2. Perceived Value Issues: Customers with Fiber Optic Internet and high monthly charges are disproportionately likely to churn.

By moving from a blanket retention strategy to a targeted approach, TelcoNow can focus its limited budget on the 914 customers identified as High Risk. This strategy minimises wasted expenditure on low-risk customers while maximising the successful retention of true churners. The model provides the essential framework for a cost-effective, data-driven retention program.

### 2. Implementation Plan and Next Steps
The following steps are recommended for the implementation of this new strategy and for advancing TelcoNow's overall predictive analytics capability.

#### A. Immediate Action Plan for Churn Mitigation �
#### Issue Personalised Offers

The Marketing/Retention team is responsible for the immediate action of issuing personalised offers. The rationale is to immediately target the 914 high-risk customers with the specific, tailored offers defined in the analysis (e.g., Contract Upgrade, Free Tech Support, Auto-Pay Discount).

#### A/B Test Offer Efficacy

The Marketing Analyst is tasked with A/B testing the efficacy of the offers. The rationale is to measure the success rate of each unique personalized offer (A vs. B vs. C) against a controlled group to accurately quantify the true Return on Investment (ROI) of the new retention strategy.

#### Integrate Risk Score

The Data Engineering team will integrate the model's risk score. The rationale is to embed the precise 0.2401 risk score into the customer relationship management (CRM) platform, which provides real-time risk visibility and enables proactive service intervention.

#### B. Future Analytical Roadmap
To further refine the retention strategy, the following analytical steps should be pursued:
1. Customer Lifetime Value (CLTV) Integration: Rerun the model specifically targeting only High-Value Customers (HVCs). This ensures that retention efforts are prioritised not just by risk, but also by the potential financial reward, further optimising marketing budget.
2. Model Comparison: Explore advanced machine learning models, such as Random Forest or Gradient Boosting Machines, to determine if prediction Accuracy and Sensitivity can be significantly improved beyond the performance of the Logistic Regression model.
3. Feature Engineering: Investigate new explanatory variables (eg. customer service call volume, historical network reliability metrics) that may further stabilise the model and improve predictive power.

## Conclusion
This capstone project successfully achieved the business task of developing a predictive model to mitigate customer churn for TelcoNow. The Exploratory Data Analysis (EDA) revealed that the typical churner has low tenure and high monthly charges, while the Logistic Regression model quantified these findings, proving that contract type is the strongest predictor of customer stability.

By transitioning from expensive, broad marketing campaigns to a targeted, personalised retention strategy, TelcoNow can significantly improve the return on investment (ROI) for its marketing budget. The final model provides the necessary risk score and customer segmentation required to implement precise, customised offers that address the root causes of cancellation—whether it is a lack of long-term commitment, service value concerns, or transactional inconvenience.

The immediate implementation of the personalised retention offers for the identified High-Risk segment is the recommended next step. Future analytical efforts should focus on integrating Customer Lifetime Value (CLTV) into the targeting criteria to ensure retention efforts are applied only to the most valuable subscribers, maximising TelcoNow’s future profits. 

## Executive Summary: TelcoNow Churn Mitigation Strategy
### Business Objective
The objective is to address a financial threat posed by customer churning of services on TelcoNow. This is achieved by developing a statistically robust predictive model in R that identifies and segments high-risk customers to optimise the marketing retention budget and minimise the company’s costly reliance on new customer acquisition.

### Key Findings & Problem Statement
The analysis confirms that the costliest errors for TelcoNow are False Negatives (failing to capture a customer who is going to churn). Through Exploratory Data Analysis (EDA) and Logistic Regression, three statistically significant risk factors are identified:
1. Contractual Instability: Month-to-month contracts are the primary churn driver.
2. Service Dissatisfaction: Users of Fiber Optic Internet show a disproportionately high risk of churning.
3. Transactional Risk: The Electronic Check payment method is strongly linked to cancellation of services.

### Churn Mitigation Strategy 
A binary classification model is built and validated, achieving an Area Under the Curve (AUC) of 0.8465. To strategically maximise the retention of true churners, a non-standard intervention threshold of 0.2401 is selected, prioritising Sensitivity over general Accuracy. This threshold translates the model's output into a precise, actionable segmentation tool.

### Strategic Recommendation
I recommend deploying a targeted, personalised retention strategy exclusively on the 914 customers classified as High Risk by the model. The offers must be tailored to address the different root causes of the risk respectively:
1. Contract Risk: Offer discounted contract upgrades (1-year minimum) for Month-to-month users.
2. Service Risk: Offer complimentary technical support and security bundles to Fiber Optic users.
3. Payment Risk: Offer an incentive (eg. $10 credit) to encourage customers to switch away from Electronic Check to automatic payment methods.

### Implementation & Action
The model is deployment-ready, and the risk score can be integrated immediately into TelcoNow's CRM system.
1. Immediate Action: Roll out the segmented offers and measure the uplift in customer retention against a control group to determine the precise Return on Investment (ROI).
2. Future Action: Subsequent analysis should integrate Customer Lifetime Value (CLTV) to further refine the target list, ensuring resources are only expended on retaining the most financially valuable high-risk subscribers.

### Summary Conclusion & Projected Impact
This data-driven approach allows TelcoNow to effectively transition away from costly, broad discounts. By focusing the marketing budget exclusively on the high-risk customers, the strategy is projected to significantly reduce customer attrition costs and stabilise the Monthly Recurring Revenue (MRR), thus achieving the company's objective of future profit maximisation.
