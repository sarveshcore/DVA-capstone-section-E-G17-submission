# **Customer Behavior Analysis & Segmentation**

**Sector:** Banking, Financial Services, and Insurance (BFSI) **Institute:** Newton School of Technology

**Team Details:**

* Ashish Kumar Yadav (2401020015)  
* Yash Agarwal (2401010511)  
* Sarvesh Srinath (2401010423)  
* Khuswant Rajpurohit (2401010228)  
* Vaibhav (2401020112)  
* Prataya Silla (2401010341)  

   ---

  ## **1\. Executive Summary**

  ### **Problem**

Financial institutions often struggle to effectively target their credit card customers due to a lack of understanding of behavioral usage patterns. Without clear segmentation, marketing efforts are generic, leading to lower engagement, churn risks, and missed opportunities for up-selling or risk mitigation. The core challenge is to transform raw transactional data into actionable customer profiles.

### **Approach**

Our team utilized the "Credit Card Customer Behavior" dataset to analyze the spending and repayment habits of 8,950 active customers. The project followed a structured data analytics lifecycle:

* **Data Quality Assurance:** We executed a rigorous cleaning process in Google Sheets, addressing missing values in `MINIMUM_PAYMENTS` and `CREDIT_LIMIT` via median imputation.  
* **Outlier Treatment:** We applied Winsorization (capping at the 99th percentile) to variables like `BALANCE` and `PAYMENTS` to prevent extreme values from distorting the analysis.  
* **Segmentation:** We derived new features such as `PURCHASE_SEGMENT` and `PAYMENT_TIER` to cluster users into behavioral groups (e.g., "Installment-Only," "Transactors").

  ### **Key Insights**

* **Distinct Behavior Groups:** We identified four primary segments: Non-Purchasers, Installment-Only, One-Off-Only, and Hybrid users.  
* **High Cash Advance Usage:** A significant 48.3% of the customer base utilizes cash advances, flagged via the `CASH_ADV_USER_FLAG`.  
* **Risk Distribution:** Analysis of `PAYMENT_TIER` reveals that while some customers maintain a "Low" risk profile (paying fully), a substantial portion falls into high-utilization bands, requiring risk mitigation strategies.

  ### **Key Recommendations**

* **Targeted Marketing:** Develop distinct credit limit offers for "Hybrid" users who frequently max out limits but maintain good repayment histories.  
* **Risk Management:** Implement stricter monitoring for customers in the "Over-Limit" utilization band (100%+) and those flagged in lower payment tiers (Tier D).  
* **Product Upsell:** Target "Installment-Only" users with EMI-based offers and "One-Off" users with rewards for higher transaction frequency.  

   ---

  ## **2\. Sector & Business Context**

  ### **Sector Overview**

The credit card industry is a highly competitive segment of the BFSI sector. Banks rely heavily on data analytics to maximize "share of wallet" and Customer Lifetime Value (CLV). With the rise of digital transactions, banks now have access to granular data on how, when, and where customers spend money.

### **Current Challenges**

1. **Generic Targeting:** Banks often use a "one-size-fits-all" approach for credit offers, which can alienate customers with specific needs.  
2. **Churn Risk:** High-value customers may leave for competitors if they feel their loyalty is not rewarded with personalized perks.  
3. **Data Quality:** Raw financial data is frequently noisy, containing missing entries or outliers that make accurate prediction difficult.

   ### **Why This Problem Was Chosen**

This project was selected to demonstrate the critical role of data cleaning and preprocessing in solving real-world business problems. By refining this dataset, we aim to simulate the workflow of a Data Analyst tasked with improving a bank's customer segmentation strategy.

---

## **3\. Problem Statement & Objectives**

### **Formal Problem Definition**

The objective is to analyze the `CC GENERAL.csv` dataset to uncover latent customer segments based on credit card usage behavior. The project involves cleaning the data, performing exploratory analysis, and applying clustering techniques to categorize customers.

### **Project Scope**

* **In-Scope:** Data ingestion, cleaning (handling nulls/outliers), Feature Engineering (creating flags/tiers), Exploratory Data Analysis (EDA), and basic customer segmentation.  
* **Out-of-Scope:** Real-time fraud detection, credit score calculation, or deployment of the model into a live banking app.

  ### **Success Criteria**

1. **Clean Dataset:** A dataset with 100% completeness (0 null values) and appropriate data types.  
2. **Actionable Segments:** Identification of distinct customer groups (e.g., Purchase Segments) with clear behavioral characteristics.  
3. **Visual Evidence:** A comprehensive dashboard or set of visualizations that highlight the key differences between these segments.  

   ---

   ## **4\. Data Description**

   ### **Exact Dataset Source**

* **Name:** Credit Card Dataset for Clustering  
* **Source:** Kaggle  
* **Link:** `https://www.kaggle.com/datasets/arjunbhasin2013/ccdata/data`

  ### **Data Structure**

* **Rows:** 8,950  
* **Columns:** 18 Original / 17 Final (after dropping `CUST_ID`)  
* **Type:** Structured CSV containing continuous and categorical financial data.

  ### **Columns Explanation**

* **CUST\_ID:** Unique ID of the customer (Categorical).  
* **BALANCE:** Balance amount left in the account.  
* **PURCHASES:** Total amount of purchases made.  
* **CASH\_ADVANCE:** Cash in advance withdrawn by the user.  
* **CREDIT\_LIMIT:** Limit of the credit card.  
* **PAYMENTS:** Total amount paid by the user.  
* **PRC\_FULL\_PAYMENT:** Percentage of full payments made.  
* **TENURE:** Duration of credit card service for the user.

  ### **Data Limitations**

* **Lack of Demographics:** The dataset does not include age, gender, or location, meaning analysis is strictly behavioral.  
* **Static Snapshot:** The data represents a 6-month window and does not account for seasonality or long-term trends beyond this period.  

  ---

  ## **5\. Data Cleaning & Preparation**

All primary cleaning and transformation steps were executed in **Google Sheets** as per capstone requirement.

### **Missing Values Handling**

An initial scan revealed missing data in two columns. We applied median imputation to avoid skewing the data with outliers.

1. **MINIMUM\_PAYMENTS:** 313 missing values (3.5%) were imputed with the median ($312.34). A flag column `MIN_PMT_IMPUTED` was added for transparency.  
2. **CREDIT\_LIMIT:** 1 missing value was imputed with the median ($3,000.00).

   ### **Outlier Treatment (Winsorization)**

To prevent extreme values from distorting the analysis, we applied Winsorization at the 99th percentile for key monetary variables:

* **BALANCE:** Capped at $9,338.80.  
* **CASH\_ADVANCE:** Capped at $9,588.16.  
* **MINIMUM\_PAYMENTS:** Capped at $9,034.10.  
* **PAYMENTS:** Capped at $13,608.72.

  ### **Feature Engineering**

We derived several new columns to enable better segmentation:

* **ZERO\_PURCHASE\_FLAG:** Flags customers with zero purchases (22.8% of data).  
* **CASH\_ADV\_USER\_FLAG:** Flags customers who used a cash advance (48.3% of data).  
* **TENURE\_BAND:** Categorizes `TENURE` into Short-Term (≤8 months), Mid-Term (9-11 months), and Full-Term (12 months).  
* **PAYMENT\_TIER:** Categorizes `PRC_FULL_PAYMENT` into discipline tiers (A, B, C, D).  
* **PURCHASE\_SEGMENT:** Creates four mutually exclusive segments: Non-Purchaser, Hybrid, Install-Only, and One-Off-Only.

  ### **Removal of Non-Predictive Features**

* **Action:** Dropped `CUST_ID` for the final analytical dataset.  
* **Rationale:** `CUST_ID` is a unique string identifier with no behavioral weight, which can interfere with clustering algorithms.  

  ---

  ## **6\. KPI & Metric Framework**

  ### **KPI Definitions**

1. **Credit Utilization Ratio:**  
   * *Formula:* `BALANCE / CREDIT_LIMIT`  
   * *Importance:* Indicates the credit risk of a customer. High utilization suggests reliance on credit or potential default risk.  
2. **Payment Ratio:**  
   * *Formula:* `PAYMENTS / MINIMUM_PAYMENTS`  
   * *Importance:* Measures repayment discipline.  
3. **Cash Advance Frequency:**  
   * *Formula:* `CASH_ADVANCE_TRX / Total Transactions`  
   * *Importance:* High frequency indicates liquidity issues for the customer, a high-revenue but high-risk behavior for the bank.  
4. **Purchase Engagement:**  
   * *Formula:* `(ONEOFF_PURCHASES + INSTALLMENTS_PURCHASES) > 0`  
   * *Importance:* Separates active spenders from dormant accounts.

   ### **Mapping KPIs to Objectives**

These KPIs are directly mapped to our objective of segmentation. For instance, `Credit Utilization` defines our Risk Segments, while `Purchase Engagement` defines our Purchase Segments.

---

## **7\. Exploratory Data Analysis (EDA)**

### **Distribution Analysis**

* **Outlier Detection:** The Interquartile Range (IQR) method identified high outlier counts in `ONEOFF_PURCHASES`(11.30%) and `CASH_ADVANCE` (11.50%). This validated the need for the Winsorization strategy applied in the cleaning phase.  
* **Skewness:** Most monetary fields were right-skewed (long tail of high spenders), which was addressed by using median-based imputation and capping.

  ### **Trend Analysis**

* **Tenure Distribution:** The majority of customers fall into the "Full-Term" (12 months) `TENURE_BAND`, indicating a stable customer base suitable for long-term behavioral analysis.  
* **Purchase Behavior:** 22.8% of the customer base made zero purchases, highlighting a significant segment of "dormant" or "cash-advance only" users.  

  ---

  ## **8\. Advanced Analysis: Segmentation**

  ### **Segmentation Strategy**

The core analysis relies on cross-tabulating customer segments to derive actionable insights. We utilized a Pivot Table approach to aggregate data based on three primary dimensions:

1. **PAYMENT\_TIER:** Categorizing customers by repayment discipline (e.g., A \- Always Full, D \- Never Full).  
2. **TENURE\_BAND:** Grouping customers by relationship length (e.g., Full-Term vs. Short-Term).  
3. **PURCHASE\_SEGMENT:** Classifying based on spending habits (Hybrid, Installments-Only, Non-Purchaser, One-Off Only).

   ### **Quantitative Metrics**

For each segment, we calculated average financial metrics to understand value and risk. These metrics include:

* **Financial Averages:** `BALANCE_CAPPED`, `CASH_ADVANCE_CAPPED`, `PAYMENTS_CAPPED`, `MIN_PAYMENTS_CAPPED`, and `CREDIT_LIMIT`.  
* **Purchase Averages:** `ONEOFF_PURCHASES` and `INSTALLMENTS_PURCHASES` to determine spending power.  
* **Frequency Averages:** `BALANCE_FREQUENCY`, `PURCHASES_FREQUENCY`, and `CASH_ADVANCE_FREQUENCY` to understand engagement levels.  

  ---

  ## **9\. Dashboard Design**

### **Dashboard Overview**

The dashboard is implemented in Google Sheets to analyze customer behavior by comparing average financial performance and transactional frequency across defined segments.

### **View Structure**

1. **Customer Count Tables:** Displays the distribution of customers (`COUNTA of CUST_ID`) segmented by `PAYMENT_TIER`, `TENURE_BAND`, and `PURCHASE_SEGMENT`.  
2. **Financial Metrics Matrix:** A detailed segmentation view by `TENURE_BAND` and `PAYMENT_TIER`. This view provides averages for key financial values like `BALANCE_CAPPED`, `CASH_ADVANCE_CAPPED`, and `CREDIT_LIMIT` to identify high-value vs. high-risk cohorts.  
3. **Frequency Analysis View:** This section segments data by `PURCHASE_SEGMENT` and provides averages for activity metrics, including `BALANCE_FREQUENCY`, `PURCHASES_FREQUENCY`, `ONEOFF_PURCHASES_FREQUENCY`, `PURCHASES_INSTALLMENTS_FREQUENCY`, and `CASH_ADVANCE_FREQUENCY`.

   ### **Interactive Elements**

* **Filters:** Users can slice data by `TENURE_BAND`, `PAYMENT_TIER`, and `CASH_ADV_USER_FLAG`.  
* **Objective:** To allow the marketing team to isolate specific cohorts (e.g., "Short-term customers who are Installment-Only") for targeted campaigns.  

  ---

  ## **10\. Insights Summary**

1. **Cash Advance Reliance:** Nearly half (48.3%) of customers use cash advances, indicating a user base that treats the credit card as a short-term loan instrument rather than just a payment tool.  
2. **Inactive Spenders:** A large portion (22.8%) are Non-Purchasers. These customers are generating revenue solely through fees or interest, or are completely dormant.  
3. **High-Risk Segment:** There is a distinct correlation between the "Over-Limit" utilization band and "Tier D" payment behavior, identifying a specific cluster of customers prone to default.  
4. **Installment Preference:** The "Installment-Only" segment represents users who likely use the card for financing electronics or appliances, distinct from day-to-day "One-Off" spenders.  

   ---

   ## **11\. Recommendations**

   ### **Strategic Actions**

1. **For Hybrid Users (High Activity):**  
   * *Action:* Increase Credit Limit.  
   * *Rationale:* These are high-value users. Increasing limits prevents them from hitting the "Over-Limit" band and encourages more spending.  
2. **For Installment-Only Users:**  
   * *Action:* Offer "No-Cost EMI" or partnership deals with electronics retailers.  
   * *Rationale:* Their behavior indicates a preference for financing over daily spending.  
3. **For Cash Advance Users:**  
   * *Action:* Promote low-interest personal loans.  
   * *Rationale:* Converting high-interest cash advances into structured personal loans can reduce default risk while retaining the customer.  
4. **For Non-Purchasers:**  
   * *Action:* Re-engagement campaigns (e.g., "Spend $50, Get $10 Cashback").  
   * *Rationale:* Incentivize the first swipe to move them into the "One-Off" segment.

   ---

## **12\. Impact Estimation**

### 

### **Business Value**

* **Revenue Growth:** Targeted upsell to "Hybrid" users could increase total transaction volume by an estimated 10-15%.  
* **Risk Reduction:** Proactive monitoring of "Tier D" customers allows the bank to freeze limits before default occurs, potentially saving 5-8% in bad debt write-offs.  
* **Efficiency:** Automated segmentation replaces manual reviews, saving analyst hours and allowing for real-time marketing triggers.  

  ---

  ## **13\. Limitations**

* **Data Issues:** The dataset required significant imputation (3.5% of Minimum Payments), which introduces synthetic data points that may slightly bias the "Tier" analysis.  
* **Assumption Risks:** We assumed that `CUST_ID` has no predictive power. If the ID contained hidden logic (e.g., sequential issuance dates), we might have lost that signal.  
* **Static Analysis:** The analysis does not account for changes in user behavior over time (e.g., a user moving from Tier A to Tier D).  

   ---

  ## **14\. Future Scope**

* **Time-Series Analysis:** With timestamped transaction data, we could analyze how customers migrate between segments over time.  
* **External Data:** Incorporating credit bureau scores (CIBIL/FICO) would vastly improve the Risk Analysis section.  
* **Predictive Modeling:** Building a classification model to predict which "One-Off" users are likely to become "Hybrid" users based on early spending patterns.  

  ---

  ## **15\. Conclusion**

This Capstone project successfully transformed raw credit card data into a strategic asset. By cleaning the data, engineering behavioral features, and applying segmentation logic, we moved from a chaotic CSV file to clear, actionable customer cohorts. The resulting insights provide a roadmap for the bank to reduce risk through better monitoring and increase revenue through targeted, segment-specific marketing offers.

---

## 

## **16\. Appendix**

### **Data Dictionary (Key Derived Fields)**

* **BALANCE\_CAPPED:** Balance variable with top 1% outliers capped.  
* **PURCHASE\_SEGMENT:** Derived text label (Non-Purchaser, Hybrid, etc.).  
* **PAYMENT\_TIER:** Derived text label (A, B, C, D) based on `PRC_FULL_PAYMENT`.

| Role | Primary Responsibility | Role/Owners Name |
| :---- | :---- | :---- |
| **Project Lead** | Timeline management, submission compliance, and ensuring the team hits the Day 3 "Data Approval" gate. | **Sarvesh Srinath** |
| **Data Lead** | Sourcing the dataset, managing the "Data Dictionary," and owning the cleaning process. | **Prataya Silla**(Sourcing) **Vaibhav**(Cleaning) |
| **Analysis Lead** | Creating Pivot Tables, complex formulas (INDEX/MATCH), and statistical checks. | **Ashish Kumar Yadav** |
| **Dashboard Lead** | Designing the final layout, color palette, and interactivity (Slicers/Charts). | **Yash Agarwal** |
| **PPT & Quality Lead** | Owning the Presentation Deck (PPT), Final Report (PDF), formatting, and verifying the Contribution Matrix. | **Khuswant Rajpurohit** |
| **Strategy Lead** | Crafting the Problem Statement, Business Recommendations, and Presentation flow. | **Ashish Kumar Yadav** |

*Declaration: We confirm that the above contribution details are accurate and verifiable through version history and submitted artifacts.*

