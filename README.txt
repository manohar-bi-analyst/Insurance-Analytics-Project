Insurance Claims Analytics Dashboard

A single-page Power BI dashboard, backed by SQL analysis, that examines claims, premium and loss ratio across policy types, age groups and gender for 10,000 insurance policies.

[Dashboard](Power%20BI%20Dashboard.png)

Overview

Insurers need a quick view of how much premium they collect, how much is claimed, and where claims are concentrated. This project answers:

1. How much premium and claim value does each policy type generate?
2. What is the loss ratio overall and by policy type?
3. How are claims split by status (settled, rejected, pending)?
4. Do claims differ by age group or gender?

Tools: SQL, Power BI Desktop, Power Query, DAX

Dataset

Source: Kaggle insurance dataset (CSV). The data is synthetic, so absolute ratios are illustrative and not real-world benchmarks.
Size: 10,004 rows reduced to **10,000 unique policies** after removing 4 exact duplicates
Columns: PolicyNumber, CustomerID, Gender, Age, PolicyType, PolicyStartDate, PolicyEndDate, PremiumAmount, CoverageAmount, ClaimNumber, ClaimDate, ClaimAmount, ClaimStatus
Policy types: Travel, Health, Auto, Life, Home

Approach

 1. Data preparation (Power Query)
* Loaded the CSV and enabled column quality, distribution and profile, using the entire dataset for profiling
* Checked for errors and blanks: `ClaimDate` is blank for all rejected claims, and their `ClaimAmount` is 0
* Removed 4 duplicate rows (10,004 to 10,000)
Added an `Age Group` column:


if [Age] <= 25 then "Young Adult"
else if [Age] <= 40 then "Adult"
else if [Age] <= 60 then "Mature"
else "Senior"


Rejected claims carry a claim amount of 0, so they add nothing to the numerator. They stay in the data so premium and customer counts remain complete.

2. DAX measures

DAX
Total Customers = DISTINCTCOUNT(InsuranceData[CustomerID])

Total Premium Amount = SUM(InsuranceData[PremiumAmount])

Total Claim Amount = SUM(InsuranceData[ClaimAmount])

Settled Claim Amount =
CALCULATE ([Total Claim Amount], InsuranceData[ClaimStatus] = "Settled")

Pending Claims =
CALCULATE(COUNTROWS(InsuranceData), InsuranceData[ClaimStatus] = "Pending")

Loss Ratio = DIVIDE ([Total Claim Amount], [Total Premium Amount])

Settled Loss Ratio = DIVIDE ([Settled Claim Amount], [Total Premium Amount])


3. Dashboard visuals
* KPI cards: Total Customers, Total Premium Amount, Total Claim Amount (settled + pending), Settled Claim Amount
* Claim amount by policy type (pie) and premium by policy type (bar)
* Claim amount by age group (column)
* Claim amount and premium by gender (clustered column)
* Loss ratio by policy type (line)

4. SQL analysis
* Queries are in [`queries.sql`](queries.sql):
* Claim status count and percentage share
* Total coverage for customers whose coverage is above the average
* Loss ratio by policy type
* Loss ratio by age group

Key findings

All figures are from the 10,000 unique policies.

Overall

| Metric | Value |
| Total premium | 5.97M |
| Total claim amount (settled + pending) | 16.90M |
| Settled claim amount | 10.11M |
| Loss ratio, all claims | 2.83 |
| Loss ratio, settled only | 1.69 |

Claim status

| Status | Claims | Share | Claim amount |
| Rejected | 4,354 | 43.5% | 0 |
| Settled | 3,386 | 33.9% | 10.11M |
| Pending | 2,260 | 22.6% | 6.80M |

Pending claims hold about 40% of total claim value. Including them lifts the loss ratio from 1.69 to 2.83.

Loss ratio by policy type

| Policy type | Policies | Loss ratio | Share of claim value |
| Travel | 4,145 | 2.86 | 41.9% |
| Health | 2,000 | 2.86 | 20.3% |
| Auto | 1,594 | 2.84 | 16.1% |
| Life | 1,248 | 2.81 | 12.2% |
| Home | 1,013 | 2.64 | 9.4% |

Travel is the largest book. Home has the lowest loss ratio, and the other four types are within 0.05 of each other.

Age groups

| Age group | % of customers | % of claim value | Loss ratio |
| Young Adult (25 and under) | 11.6% | 11.5% | 2.86 |
| Adult (26 to 40) | 21.7% | 21.1% | 2.77 |
| Mature (41 to 60) | 28.5% | 29.7% | 2.92 |
| Senior (over 60) | 38.2% | 37.8% | 2.78 |

Claims follow customer share in every age group. Seniors are 38.2% of customers and 37.8% of claim value, so age alone does not separate high-risk from low-risk customers in this dataset.

Gender
Claims (8.5M male, 8.4M female) and premium (3.0M each) are almost identical.

Recommendations

* Review the 2,260 pending claims (6.8M): they are the largest uncertainty in the loss ratio, so faster settlement and reserve review would give a clearer picture
* Investigate the 43.5% rejection rate: break it down by policy type and rejection reason to see whether it reflects underwriting quality or claims-process friction
* Do not price on age alone: loss ratios are close across age groups, so combine age with other risk factors
* Look at Home: its lower loss ratio (2.64) may point to better risk selection worth studying

Limitations

The dataset is synthetic. Claim amounts do not move with premium, which is why the loss ratio is far above 1. The project demonstrates the method, not real underwriting results.
* The loss ratio uses full-year `PremiumAmount` as a proxy for earned premium. A production version would use earned premium for the same period as the losses.
* Pending claims are included in the 2.83 ratio because their final outcome is unknown. The settled-only ratio (1.69) is shown alongside.

Possible extensions

* Rejection rate by policy type, age group and gender
* Settlement time analysis (needs a settlement date, which this dataset lacks)
* Earned-premium-based loss ratio by policy month

Repository contents
* `InsuranceData.csv`: source data
* `queries.sql`: SQL analysis
* `Power BI Dashboard.png`: dashboard screenshot
* `Insurance Analytics.pbix`: Power BI report
