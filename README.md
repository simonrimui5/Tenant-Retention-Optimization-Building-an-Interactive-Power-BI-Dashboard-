# Tenant Retention Optimization: Building an Interactive Power BI Dashboard for Residential Real Estate Excellence
![dillon-kydd-2keCPb73aQY-unsplash](https://github.com/user-attachments/assets/74ec7fec-d5b0-4602-9d4d-cdaadfb6beef)
## Table Of Content
- [Project Overview](#project-overview)
- [Rationale for the Project](#rationale-for-the-project)
- [Aim of the Project](#aim-of-the-project)
- [Data Description](#data-description)
- [Tech Stack](tech-stack)
- [Project Scope](#project-scope)
- [Data Analysis and transformation](#data-analysis-and-transformation)
- [Combining Tables](#combining-tables)
- [Renaming Tables](#renaming-tables)
- [Creating Tables](#creating-tables)
- [Creating a Measures Table](#creating-a-measures-table)
- [New Measures](#new-measures)
- [New Columns](#new-columns)
- [Visualization Implementation in Power BI](#visualization-implementation-in-power-bi)
- [Card Visualizations](#card-visualizations)
- [KPIs being monitored](#kpis-being-monitored)
- [Detailed Visualization Overview](#detailed-visualization-overview)
- [Line and Clustered Column Chart](#line-and-clustered-column-chart)
- [Clustered Bar Chart](#clustered-bar-chart)
- [Stacked Column Chart](#stacked-column-chart)
- [Donut Chart](#donut-chart)
- [Trend Lines](#trend-lines)
- [DrillThrough Buttons](#drill-through-buttons)
- [Slicers](#slicers)
- [Tenant Retention Dashboard](#tenant-retention-dashboard)
- [Lease Term Details](#lease-term-details)
- [Satisfaction Score Details](#satisfaction-score-details)
- [Churn Rate Details](#churn-rate-details)
- [Insights](#insights)
- [Recommendations](#recommendations)
## Project Overview
HomeVibe Properties has identified several pressing challenges related to tenant retention, presented in bullet points for clarity:
- High Tenant Churn Rates: The company faces elevated tenant turnover rates, resulting in increased operational costs and lost revenue. 
- Limited Tenant Insights: HomeVibe Properties lacks actionable insights into tenant satisfaction and their concerns, making it challenging to proactively address issues. 
- Inefficient Lease Renewals: The lease renewal process is plagued by inefficiencies, contributing to tenant dissatisfaction and administrative overhead. 
- Lack of Trend Identification: Identifying trends and patterns affecting tenant retention is difficult, hindering the development of effective retention strategies. 
## Rationale for the Project
The importance of tenant retention in the real estate and property management industry cannot be overstated. Here are the top five reasons highlighting the significance of this project:
- Enhanced Revenue: Improved tenant retention directly correlates with increased revenue and profitability as existing tenants renew their leases. 
- Positive Reputation: Elevated tenant satisfaction generates positive word-of-mouth, attracting new tenants and enhancing the company's brand image. 
- Operational Efficiency: Streamlining lease renewal processes reduces administrative overhead, allowing for better resource allocation. 
- Competitive Edge: Data-driven decision-making provides a competitive advantage in the highly competitive real estate market. 
- Sustainability: A focus on tenant retention aligns with sustainable business practices by reducing the environmental impact of turnover. 
## Aim of the Project
The primary objectives of this project are as follows:
- Interactive Tenant Retention Dashboard: Design and implement an interactive tenant retention dashboard using Power BI. 
- Data Analysis: Analyze historical tenant data to identify factors affecting tenant churn and provide actionable insights. 
- Lease Renewal Optimization: Streamline lease renewal processes to increase tenant retention rates. 
## Data Description
This case study contains 4 datasets and  they are as follows;
### Tenant Information Dataset:
- Tenant ID (N/A): A unique identifier for each tenant. 
- Tenant Name (Text): The name of the tenant. 
- Contact Details (Text - Phone number or email): Contact information for the tenant. 
- Lease Start Date (Date): The date when the tenant's lease agreement started. 
- Lease End Date (Date): The date when the tenant's lease agreement is scheduled to end. 
### Lease Details Dataset:
- Lease ID (N/A): A unique identifier for each lease. 
- Lease Start Date (Date): The date when the lease agreement started. 
- Lease End Date (Date): The date when the lease agreement is scheduled to end. 
- Lease Term (Months) (Months): The duration of the lease in months. 
- Rent Amount (Currency - e.g., USD): The amount of rent for the lease. 
- Payment History (Currency - e.g., USD): The total amount paid for the lease. 
### Tenant Feedback Dataset:
- Feedback ID (N/A): A unique identifier for each feedback entry. 
- Tenant ID (Link to Tenant Information Dataset): Identifies the tenant associated with the feedback. 
- Survey Response (Text): Text responses to survey questions, indicating tenant satisfaction. 
- Comments (Text): Open-ended comments provided by tenants, offering additional feedback or details. 
### Property Information Dataset:
- Property ID (N/A): A unique identifier for each property.
- Property Name (Text): The name or title of the property.
- Location (Text - Address or coordinates): The location of the property.
- Property Type (Text): The type of property (e.g., Apartment, Single-family home).
- Amenities (Text): A list of amenities offered at the property.
- Historical Occupancy Rate (%) (Percentage): The historical occupancy rate of the property, expressed as a percentage. 
## Tech Stack
Tool - Power BI
It will be used for ;
- Data Integration
- Data Transformation
- Data Analysis and Modeling
- Real-time Data Visualization
- Dashboard Design
- Reporting
- Cloud Integration 
## Project Scope
- Exploratory Data Analysis: Explore the data to understand its characteristics and discover patterns.
- Data Transformation: Prepare the data for analysis by transforming, encoding, or normalizing it.
- Data Analysis: Analyze data to understand pattern in order to generate insights that will be visualized.
- Data Visualization: Create visual representations to communicate insights effectively.
- Interpretation and Insight Generation: Extract meaningful insights and interpret the results.

## Data Analysis and transformation
### Data Transformation

### Combining Tables
The Tenant Information Dataset and Tenant Feedback Dataset were merged into a new table named Tenant Information and Feedback using Tenant ID as the key.
This facilitated analyzing tenant satisfaction alongside tenant details.
```DAX
Tenant Information and Feedback = 
NATURALINNERJOIN('Tenant Information Dataset', 'Tenant Feedback Dataset')
```
### Renaming Tables
The Lease Details Dataset was renamed to Full Data to simplify referencing.
### Creating Tables
I created two tables, Start Date and End Date, using the ADDCOLUMNS function to generate a comprehensive date range (2020–2025) and add metadata for time-based analysis.
```DAX
Start Date = 
ADDCOLUMNS(
    CALENDAR("2020-01-01", "2025-12-31"),
    "Year", YEAR([Date]),
    "Month", MONTH([Date]),
    "Day", DAY([Date]),
    "MonthName", FORMAT ([Date],"MMMM"),
    "DayOfWeek", WEEKDAY([Date]),
    "DayName", FORMAT ([Date], "dddd")
```
```DAX
End Date = 
ADDCOLUMNS(
    CALENDAR("2020-01-01", "2025-12-31"),
    "Year", YEAR([Date]),
    "Month", MONTH([Date]),
    "Day", DAY([Date]),
    "MonthName", FORMAT([Date], "MMMM"),
    "DayOfWeek", WEEKDAY([Date]),
    "DayName", FORMAT([Date], "dddd")
)
```
### Creating a Measures Table
A Measures Table was introduced to organize all calculated measures.
```DAX
Measures Table = DATATABLE(
    "DummyColumn", STRING,
    {{"DummyValue"}}
)
```
## New Measures
### Churn Rate
Calculated the proportion of leases that ended within the past year compared to total leases.
```DAX
Churn Rate = 
CALCULATE(
    DIVIDE(
        COUNTROWS(FILTER('Full Data', 'Full Data'[Lease End Date] < TODAY() && 'Full Data'[Lease End Date] > TODAY() - 365)),
        COUNTROWS('Full Data')
    )
)
```
### Total Expected Rent
Estimated the total expected rent revenue based on lease terms.
```DAX
Total Expected Rent = 
SUMX('Full Data', 'Full Data'[Rent Amount (USD)] * 'Full Data'[Lease Term (Months)])
```
### Rent Collection Performance
Assessed the ratio of total payments received to total expected rent.
```DAX
Rent Collection Performance = 
DIVIDE([Total Payment History], [Total Expected Rent])
```
### Current Occupancy Rate
Measured the current occupancy rate based on active leases.
```DAX
Current Occupancy Rate = 
DIVIDE(
    CALCULATE(COUNTROWS('Full Data'), 'Full Data'[IsActiveLease] = 1),
    CALCULATE(COUNTROWS('Full Data'), ALL('Full Data'[Lease Start Date]), ALL('Full Data'[Lease End Date]))
)
```
## New Columns
### Satisfaction Score
A column was added to the Tenant Information and Feedback table to assign numeric values based on survey responses.
```DAX
Satisfaction Score = SWITCH(
    TRUE(),
    'Tenant Information and Feedback'[Survey Response] = "Very Satisfied", 5,
    'Tenant Information and Feedback'[Survey Response] = "Satisfied", 4,
    'Tenant Information and Feedback'[Survey Response] = "Neutral", 3,
    'Tenant Information and Feedback'[Survey Response] = "Dissatisfied", 2,
    'Tenant Information and Feedback'[Survey Response] = "Very Dissatisfied", 1,
    0 // Default value
)
```
### Historical Occupancy Rate Decimal
Added a column in the Property Information Dataset to convert the percentage occupancy rate into a decimal format for easier calculation.
```DAX
Historical Occupancy Rate Decimal = 
'Property Information Dataset'[Historical Occupancy Rate (%)] / 100
```
### IsActiveLease
Added a column in the Full Data table to flag active leases.
```DAX
IsActiveLease = 
IF('Full Data'[Lease End Date] >= TODAY(), 1, 0)
```
## Visualization Implementation in Power BI
After data transformation and the creation of DAX measures, the interactive dashboard was designed with the following features to ensure a user-friendly interface and actionable insights:

### Card Visualizations
Purpose: Display key performance indicators (KPIs) at a glance.

### KPIs being monitored
- Churn Rate
- Average Satisfaction Score
- Average Occupancy Rate
- Average rent
- % rent rate

## Detailed Visualization Overview
The dashboard features a variety of visualizations and interactive elements, tailored to analyze tenant retention and real estate performance effectively:

### Line and Clustered Column Chart
### Purpose:
Compare churn status and satisfaction scores across different property types.
### Visualization Details:
Columns: Churn Status 
Line:  Satisfaction Score
X-axis: Property Type
Y-axis (Primary): Satisfaction Score
Y-axis (Secondary): Churn Rate (%)
### Clustered Bar Chart
### Purpose: 
Break down satisfaction scores by survey responses to identify tenant sentiment trends.
### Visualization Details:
Bars: Satisfaction Score
Categories: Survey Responses
X-axis: Survey Responses
Y-axis: Count of Tenant Feedback Entries
### Stacked Column Chart
### Purpose: 
Visualize the frequency of lease terms across various durations.
### Visualization Details:
Columns: Lease Term Frequency
Categories: Lease Terms 
X-axis: Lease Term (Months)
Y-axis: Frequency (Number of Leases)
### Donut Chart
### Purpose:
Highlight the proportion of renewed vs. expired leases.
Renewed Leases
Expired Leases
Values: Count of leases for each status
Labels: Display percentages for clear insights.
### Trend Lines
### Purpose: 
Showcase temporal patterns to monitor tenant retention and property performance metrics.
### Trendlines Implemented:
### Churn Status by Month:
X-axis: Month
Y-axis: % of Churned Tenants
### Renewed vs. Expired Leases by Month:
X-axis: Month
Y-axis: Lease Status Count (Renewed, Expired)
### Average Satisfaction Score by Month:
X-axis: Month
Y-axis: Satisfaction Score (Average)
### Rent Collection Performance by Month:
X-axis: Month
Y-axis: Performance Metric
### Drill-Through Buttons
### Purpose: 
Facilitate granular exploration of specific insights.
### Drill Through Implementations:
### Churn Rate Details:
Navigates to a page with tenant details contributing to churn.
### Lease Term Details:
Detailed view of lease term distributions and their impact.
### Satisfaction Score Details:
Displays detailed feedback and satisfaction trends.
### Slicers
### Purpose:
Provide dynamic filtering for enhanced analysis.
### Slicers Used:
### Month: 
Filter data for specific months.
### Year: 
Toggle between years for year-over-year analysis.
### Property Type: 
Analyze specific property types.
## Tenant Retention Dashboard
![Screenshot 2024-11-29 172711](https://github.com/user-attachments/assets/e683e914-c879-4c6e-9d0c-14927831552b)

## Lease Term Details
![Screenshot 2024-11-29 172927](https://github.com/user-attachments/assets/0236d1f0-de14-47d5-b0b7-485db82fb237)

## Satisfaction Score Details
![Screenshot 2024-11-29 172954](https://github.com/user-attachments/assets/fc390d5f-17d4-4bec-9d06-8052e1f88eec)
## Churn Rate Details
![Screenshot 2024-11-29 173021](https://github.com/user-attachments/assets/5202fb54-7327-49ff-9c4d-72a86ce9df2a)
## Insights 
- Seasonal and Trend-Based Churn Patterns- the line chart tracking churn rates by month reveals clear seasonal patterns, highlighting specific times of the year when churn rates peak. These insights can guide targeted retention strategies during high-risk periods.
- Lease Term Preferences and Their Impact on Retention- analysis of lease term frequency distributions shows how tenant preferences—whether for shorter or longer leases—correlate with retention rates. This insight suggests opportunities to adjust lease offerings to align with tenant behaviors and improve retention.
- Direct Correlation Between Tenant Satisfaction and Retention- The bar chart comparing satisfaction scores by property type underscores the critical role tenant satisfaction plays in retention. Properties with higher satisfaction scores consistently exhibit lower churn rates, providing actionable data to reduce tenant turnover.
- Financial Efficiency’s Impact on Property Performance- Insights into rent collection performance highlight operational strengths or weaknesses affecting the company’s financial health. Addressing these issues could improve the ability to invest in property enhancements and tenant services, boosting overall performance.
- Effectiveness of Renewal Strategies Over Time- A longitudinal comparison of renewed versus expired leases measures the success of retention strategies. Trends can reveal whether recent efforts are effective or if adjustments are necessary to retain more tenants.
## Recommendations
- Implement Targeted Retention Programs- Leverage churn pattern insights to design focused retention initiatives during high-risk periods. Examples include personalized outreach, special promotions, or tenant engagement events to strengthen community ties.
- Adjust Lease Terms to Align with Tenant Preferences- If data indicates a strong link between specific lease lengths and retention rates, offer flexible or preferred lease options to encourage longer tenancy, catering to tenant preferences.
- Enhance Tenant Satisfaction- Focus on improving key areas impacting tenant satisfaction, such as property amenities, maintenance response times, and community-building activities. These efforts can directly address churn drivers and foster loyalty.
- Optimize Financial Operations- Resolve rent collection inefficiencies by introducing technology-based payment solutions or incentives for timely payments. Improving these processes can enhance financial stability and fund tenant-focused initiatives.
- Refine and Monitor Renewal Strategies- Continuously evaluate renewal versus expiration trends to measure the impact of retention 






