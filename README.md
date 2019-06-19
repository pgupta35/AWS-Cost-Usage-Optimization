# AWS Cost and Usage Billing Optimization

**Usecase to Optimize the AWS Cost &amp;  Usage Billing reports to clients using AWS Redshift, S3 and Quicksight**

**Usecases**:
- Exporting AWS Partner Billing and Cost Report CSV/SQL/ZIP from S3 Bucket. 
- Import into Amazon Redshift for Querying the reports data through SQL commands.
- Import into Amazon QuickSight for generating Customized Analytics Chart Report with data filtering.

**Deliverables**:
- Wring SQL Queries for generating bill usage and Cost report for various Customer using Redshift and Creating Reports using QuickSight.

1. Import AWS Partner Billing and Cost Report in CSV format into Amazon Redshift for Querying the reports data through SQL commands

<details>
<summary>Usecase Workflow</summary>

**Step 1: Upload the sample file (Sample Partner CUR.csv) to an Amazon S3 bucket:**
  <br>
 <img src="https://github.com/sahanasj/AWS-Cost-Usage-Optimization/blob/master/AWS-Partner-Billing-Reports-Screenshots/Upload-AWS_Partner-Billing-report-to-Amazons3.PNG">

**Step 2: Access Redshift using PSQL client:**
<br>
<img src="https://github.com/sahanasj/AWS-Cost-Usage-Optimization/blob/master/AWS-Partner-Billing-Reports-Screenshots/Login-into-Redshift.PNG">

**Step 3: Create the Data table structure for the given CSV​ on Redshift Database:**
<br>
<img src="https://github.com/sahanasj/AWS-Cost-Usage-Optimization/blob/master/AWS-Partner-Billing-Reports-Screenshots/AWS-Create-a-Table-in-Redshift.PNG">

**Data Table Description:**
<img src="https://github.com/sahanasj/AWS-Cost-Usage-Optimization/blob/master/AWS-Partner-Billing-Reports-Screenshots/Table-Description.PNG">

**Step 4: Import AWS Partner Billing and Cost Report into Redshift Database using COPY Command through PSQL client CLI.**
<img src="https://github.com/sahanasj/AWS-Cost-Usage-Optimization/blob/master/AWS-Partner-Billing-Reports-Screenshots/aws-copy-datafiles-to-redshiftDB.PNG">

[Or]

**Import the AWS billing report through PSQL GUI:**
<img src="https://github.com/sahanasj/AWS-Cost-Usage-Optimization/blob/master/AWS-Partner-Billing-Reports-Screenshots/Import-CSV-Datafile-into-PSQL-Client-through-GUI.PNG">

**Step 5: Verify the Loaded the AWS Cost and Billing Data into Redshift:**

<img src="https://github.com/sahanasj/AWS-Cost-Usage-Optimization/blob/master/AWS-Partner-Billing-Reports-Screenshots/Load-Sample-Billing-datafile-to-Redshift-DB.PNG">

**Step 6: SQL Queries to filter the Data Table:**

SQL Queries List in AWS:
<img src="https://github.com/sahanasj/AWS-Cost-Usage-Optimization/blob/master/AWS-Partner-Billing-Reports-Screenshots/SQL-Queries-List-on-AWS.PNG">

SQL Query to filter the Data Tables by **lineItem_UsageAccountID, lineItem_ProductCode, lineItem_UsageType, lineItem_UsageAmount.**

<img src="https://github.com/sahanasj/AWS-Cost-Usage-Optimization/blob/master/AWS-Partner-Billing-Reports-Screenshots/SQL-Query-To-Redshift-To-Filter.PNG">

[or]

Through PSQL GUI:
<img src="https://github.com/sahanasj/AWS-Cost-Usage-Optimization/blob/master/AWS-Partner-Billing-Reports-Screenshots/SQL-Query-To-Filter-Data.PNG">

<img src="https://github.com/sahanasj/AWS-Cost-Usage-Optimization/blob/master/AWS-Partner-Billing-Reports-Screenshots/SQL-Query-To-filter-Datatables-in-RedshiftDB.PNG">

SQL Query to limit the Data Tables with 10 entries:

<img src="https://github.com/sahanasj/AWS-Cost-Usage-Optimization/blob/master/AWS-Partner-Billing-Reports-Screenshots/SQL-Query-OrderBy-UserID.PNG">

**Step 6: Export/Unloads the result of SQL queries to one or more files in CSV format on Amazon Simple Storage Service**

<img src="https://github.com/sahanasj/AWS-Cost-Usage-Optimization/blob/master/AWS-Partner-Billing-Reports-Screenshots/unload_command_to_export_filtered_data_to_aws_s3.PNG">

</details>

<details>
<summary><b>Postgresql Queries</b></summary>

**To filter the AWS costs based on Product Code and Availability zone:**
<br>
**select lineitem_productcode, lineitem_availabilityzone, sum(cast(lineitem_unblendedcost as float)) from AWSBillingPartner_Test where lineitem_availabilityzone <> '' group by lineitem_availabilityzone, lineitem_productcode;**
<br>
<img src="https://github.com/sahanasj/AWS-Cost-Usage-Optimization/blob/master/AWS-Partner-Billing-Reports-Screenshots/2-SQL-Query-Calculate-the-cost-based-on-awsEC2usage.PNG">

**To filter the AWS costs based on UserAccountID, Usage Type Group and Availability zone:
<br>
select DISTINCT lineitem_usageaccountid as User_Account, lineitem_productcode as Product_Usage, lineitem_availabilityzone as User_AZ, sum(cast(lineitem_unblendedcost as float)) as Userusage_cost from AWSBillingPartner_Test where lineitem_usageaccountid IS NOT NULL and lineitem_availabilityzone IS NOT NULL group by lineitem_availabilityzone, lineitem_productcode, lineitem_usageaccountid;**
<br>
<img src="https://github.com/sahanasj/AWS-Cost-Usage-Optimization/blob/master/AWS-Partner-Billing-Reports-Screenshots/3-SQL-Query-filter-based-on-resources.PNG">

**To filter the AWS costs based AWS S3 service usage:
<br>
select sum(cast(lineitem_unblendedcost as float)) as user_usagecost from AWSBillingPartner_Test where lineitem_productcode='AmazonS3' or pricing_unit='GB';**
<br>
<img src="https://github.com/sahanasj/AWS-Cost-Usage-Optimization/blob/master/AWS-Partner-Billing-Reports-Screenshots/2-SQL-Query-Calculate-the-cost-based-on-awss3usage.PNG">


**To filter the AWS costs based AWS EC2 service usage:
<br>
select lineitem_usageaccountid as user_accountid, sum(cast(lineitem_unblendedcost as float)) as user_usagecost from AWSBillingPartner_Test where lineitem_productcode='AmazonEC2' group by lineitem_usageaccountid;**
<br>
<img src="https://github.com/sahanasj/AWS-Cost-Usage-Optimization/blob/master/AWS-Partner-Billing-Reports-Screenshots/2-SQL-Query-Calculate-the-cost-based-on-awsEC2usage.PNG">

**To calculate the Cost based on the Services and utilization date:
<br>
select sum(cast(lineitem_unblendedcost as float)) from AWSBillingPartner_Test where lineitem_productcode='AmazonS3' or lineitem_productcode='AmazonEc2' and bill_BillingPeriodEndDate > '2018-03-01';**

<img src="https://github.com/sahanasj/AWS-Cost-Usage-Optimization/blob/master/AWS-Partner-Billing-Reports-Screenshots/3-SQL-Query-Calculate-the-cost-based-on-services_%26_date.PNG">

**'Unloads' the SQL query result to file on Amazon Simple Storage Service  (Amazon S3) and download as CSV file.
<br>
<img src="https://github.com/sahanasj/AWS-Cost-Usage-Optimization/blob/master/AWS-Partner-Billing-Reports-Screenshots/unload_command_to_export_filtered_data_to_aws_s3.PNG">

**UNLOAD automatically creates encrypted file on AWS S3:**
<br>
<img src="https://github.com/sahanasj/AWS-Cost-Usage-Optimization/blob/master/AWS-Partner-Billing-Reports-Screenshots/download_customized_data_as_csv_format.PNG">

**Download the customized file in CSV format from AWS S3: (Attached sample CSV file in the mail thread)**
<br>
<img src="https://github.com/sahanasj/AWS-Cost-Usage-Optimization/blob/master/AWS-Partner-Billing-Reports-Screenshots/download_customized_data_as_csv_format-to-s3.PNG">
<br>
<img src="https://github.com/sahanasj/AWS-Cost-Usage-Optimization/blob/master/AWS-Partner-Billing-Reports-Screenshots/download_customized_data_in_csv_format_.PNG">

</details>
  





