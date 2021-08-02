# AWS Billing and Cost Management

## Cost Explorer

- Tracks and analyzes your AWS usage. It is free for all accounts
- Includes a default report that helps visualize the costs and usage associated with our TOP FIVE cost-accruing AWS services, and gives you a detailed breakdown on all services in the table view
- We can view data for up to last 12 months, forecast how much we are likely to spend for the next tree months and get recommendations on what Reserved Instances to purchase
- Cost Explorer must be enabled before it can be used. The owner of the account can enable it

## AWS Cost and Usage Reports

-  AWS Cost and Usage report provides information about our usage of AWS resources and estimated costs for that usage
- The AWS Cost and Usage report is a `.csv` file or a collection of `.csv` files that is stored in an S3 bucket. Anyone who has permissions to access the specified S3 bucket can see the billing report files
- We can use the Cost and Usage report to track your Reserved Instance Utilization, charges, and allocations
- For time granularity, we can choose one of the following:
    - Hourly: if we want our items in the report to be aggregated by the hour
    - Daily: if we want our items in the report to be aggregated by the day
    - Monthly: if we want our items in the report to be aggregated by month
- Report can be automatically uploaded into AWS Redshift and/or AWS QuickSight for analysis

## AWS Budgets

- Allows us to set custom budgets that will alert us when our costs or usage exceed or are forecasted to exceed your budgeted amount
- With Budgets, we can view the following information:
    - How close our plan is to our budgeted amount or to the free tier limits
    - Our usage to date, including how much you have used of your Reserved Instances and purchased Savings Plans
    - Our current estimated charges from AWS and how much your predicted usage will incur in charges by the end of the month
    - How much of our budget has been used
- Budget information is updated up to three times a day
- Types of Budgets:
    - **Cost budgets**: plan how much we want to spend on a service
    - **Usage budgets**: plan how much we want to use one or more services
    - **RI utilization budgets**: define a utilization threshold and receive alerts when your RI usage falls below that threshold
    - **RI coverage budgets**: define a coverage threshold and receive alerts when the number of your instance hours that are covered by RIs fall below that threshold
- Budgets can be tracked at the daily, monthly, quarterly, or yearly level, and we can customize the start and end dates
- Budget alerts can be sent via email and/or Amazon SNS topic
- First two budgets created are free of charge