
Insurance Aanlytics dashboard

This project demonstrates how data analytics can be applied in the insurance domain to improve decision making and operational efficiency. Using SQL and Power BI the project analyzes claims data to identify fraud patterns, track settlement timelines, and measure customer retention.


# Insurance Analytics Dashboard

### Dashboard Link : https://app.powerbi.com/groups/me/reports/0c6ea651-af9f-40db-805a-01944b4da7bb/aecbfc671b20b880e3dd?experience=power-bi

## Problem Statement

Insurance carriers face critical challenges in managing claims, detecting fraud, and retaining customers.  
Traditional reporting methods often fail to provide timely insights, leading to delayed claim settlements, undetected fraudulent activities, and poor customer satisfaction.

The lack of structured analytics results in:

High fraud risk → fraudulent claims increase operational losses.

Inefficient claims processing → longer settlement cycles reduce trust.
Customer churn → inability to identify at‑risk policyholders impacts retention.

Limited underwriting accuracy → poor risk assessment affects profitability. 

From the inference of this project it clearly states age > 60 is a high risk group for insurers due to health deterioration, chronic illnesses, and higher hospitalization rates.

This insight can feed into risk scoring models and premium adjustments.

### Steps followed 

- Step 1 : Load data into Power BI Desktop, dataset is a csv file.
- Step 2 : Open power query editor & in view tab under Data preview section, check "column distribution", "column quality" & "column profile" options.
- Step 3 : Also since by default, profile will be opened only for 1000 rows so you need to select "column profiling based on entire dataset".
- Step 4 : It was observed that in none of the columns errors & empty values were present except column named "Arrival Delay".
- Step 5 : In the report view, under the view tab, theme was selected.
- Step 6 : Three card visuals were added to the canvas, representing total claims paid, total premium amount, no of customers.
- Step 7 : A bar chart was also added to the report design area representing total claims mady by the different age groups. 
  
In our dataset, in the claims date parameters was assigned value 0, representing no claims were made as they were rejected.
All these values have been ignored while performing the calculations.

- Step 14 : Calculated column was created in which, customers were grouped into various age groups.

for creating new column we have used the condition column from the add column section;
       
        Age Group = 
        
        if age <=25 then 'Young Adult',
        
        if age <=40 then 'Adult'

        if age <=60 then 'Mature'

        else 'old'
        
Snap of new calculated column ,

![Snap_1] https://github.com/manohar-bi-analyst/Insurance-Analytics-Project/blob/main/Screenshot%202026-09-26%20020225.png

        
- Step 15 : New measure was created to find total count of customers.

Following DAX expression was written for the same,
        
       Total Premium Amount = SUM(InsuranceData[PremiumAmount])
        
A card visual was used to represent Total Premium Amount.

![Snap_Count]https://github.com/manohar-bi-analyst/Insurance-Analytics-Project/blob/main/Screenshot%202026-09-26%20020552%201.png

        
 - Step 16 : New measure was created to find  % of loss ratio,
 
 Following DAX expression was written to find % of loss ratio,
 
         Loss ratio = DIVIDE(SUM(InsuranceData[ClaimAmount]),SUM(InsuranceData[PremiumAmount]))
 
 A line chart was used to represent  loss ratio by different product type.
 
 Snap of % of customers who preferred business class
 
 ![Snap_Percentage]https://github.com/manohar-bi-analyst/Insurance-Analytics-Project/blob/main/Screenshot%202026-09-26%20020934%202.png

 
 - Step 17 : New measure was created to calculate total claim amount by the customers.
 
 Following DAX expression was written to find total distance,
 
         Total Claim Amount = SUM(InsuranceData[ClaimAmount])
    
 A card visual was used to represent this Total Claim Amount.
 
 
 ![Snap_3]https://github.com/manohar-bi-analyst/Insurance-Analytics-Project/blob/main/Screenshot%202026-09-26%20021223%203.png
 - Step 18 : The report was then published to Power BI Service.
 

# Snapshot of Dashboard (Power BI Service)

![dashboard_snapo]https://github.com/manohar-bi-analyst/Insurance-Analytics-Project/blob/main/Power%20BI%20Dashboard.png

Use of SQL

 Find the Total count and percentage share of Calim status where in for rejected and settled claims.

 ![snap_1]https://github.com/manohar-bi-analyst/Insurance-Analytics-Project/blob/main/Sql%20Screenshot%201.png

 Finf the total coverage amount for the customers whose coverage is greater than the average coverage amount.

![snap_2]https://github.com/manohar-bi-analyst/Insurance-Analytics-Project/blob/main/Sql%20Screenshot%202.png

# Insights from 

A single page report was created on Power BI Desktop & it was then published to Power BI Service.

High Claims Among Senior Citizens (Age > 60)

      Customers above 60 years contribute disproportionately to total claims.

      Business Impact: Insurers should design age - specific products, allocate higher reserves, and introduce preventive wellness programs for senior citizens.

    
High Claim Frequency

     Travel policies often cover multiple risks (trip cancellations, medical emergencies abroad, lost baggage, flight delays).

     This leads to more frequent claim submissions compared to single - risk products like accident or health.

Interpretation:

     A loss ratio above 1.0 means insurers are paying out more in claims than they collect in premiums.

     Ratios in the 2.64 - 2.86 range indicate significant underwriting losses — claims are more than double the premiums collected.

     Adjust premiums, redesign products, or introduce preventive wellness programs to reduce claim frequency.
