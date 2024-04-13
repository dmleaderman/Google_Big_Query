# Google_Big_Query
Google Big Query
# BigQuery

## Introduction to BigQuery
### Data Warehouse History
#### Databases
- Primarily used for transaction processing
- Difficult for managers to analyze data and create reports when the data resides in numerous databases across an organization (log files, streaming data, etc.)

#### Data warehouses
- Collect data from wide variety of sources
- Designed for reporting and data analysis

#### Why Use BigQuery
- Ease of implementation
  - Building our own is expensive, time-consuming, and difficult to scale
  - With BigQuery, just load data and pay only for what you use
- Speed
  - Processes billions of rows in seconds
  - Handles real-time analysis of streaming data

### View BigQuery Public Datasets
```shell
SELECT name, state, year FROM `bigquery-public-data.usa_names.usa_1910_2013` LIMIT 100;

SELECT name, state , year FROM `bigquery-public-data.usa_names.usa_1910_2013`;
```

### Running a Query in GCP Console
1. Login to GCP, go to the console, then select **BigQuery** from the menu or type **BigQuery** in the search bar

![bq3](../../image/bq3.PNG)

2. Click **+ Compose a new query**
3. In the query editor, enter a valid GoogleSQL query, then click **Run**
   - **Query 1**
   ```
    SELECT 
      *
    FROM
      `bigquery-public-data.usa_names.usa_1910_2013`
    ORDER BY
      number DESC
    LIMIT
      10
   ```

   ![bq4](../../image/bq4.PNG)

   - **Query 2**
   ```shell
   SELECT
     *
   FROM
     `bigquery-public-data.usa_names.usa_1910_2013`
   WHERE
     gender = 'F'
   ORDER BY
     number DESC
   LIMIT
     10
   ```
   
   ![bq5](../../image/bq5.PNG)

   - **Query 3** Query the  most common names in the United States between the years 1910 and 2013
   ```
   SELECT
      name, gender,
      SUM(number) AS total
   FROM
      `bigquery-public-data.usa_names.usa_1910_2013`
   GROUP BY
      name, gender
   ORDER BY
      total DESC
   LIMIT
    10;

   ```

   ![bq6](../../image/bq6.PNG)

### View BigQuery Results 
#### Job Information
![bq8](../../image/bq8.PNG) <br>
#### Results
![bq9](../../image/bq9.PNG) <br>
#### Json Results
![bq10](../../image/bq10.PNG) <br>
#### Execution Details
![bq11](../../image/bq11.PNG) <br>
#### Chart Preview
![bq12](../../image/bq12.PNG) <br>

### Perform a Dry Run
A dry run in BigQuery provides the following information:

- estimate of charges in on-demand mode
- validation of your query
- approximate size and complexity of your query in capacity mode

Dry runs don't use query slots, and you are **not charged for performing a dry run**. 
You can use the estimate returned by a dry run to calculate query costs in the pricing calculator.

- Go to the BigQuery page
- Enter your query in the query editor.
  If the query is valid, then a check mark automatically appears along with the amount of data 
  that the query will process. 


  ![bq14](../../image/bq14.PNG)

The following query will process **6.2 GB** when run.
```shell
SELECT COUNT(*) FROM `patents-public-data.google_patents_research.publications_202101` WHERE title LIKE "%covid%";
```

The follow query This query will process **323.61 GB** when run.
```shell
SELECT * FROM `patents-public-data.google_patents_research.publications_202101` WHERE title LIKE "%covid%";
```

### BigQuery Pricing
BigQuery pricing has two main components:
- **Compute** (analysis) pricing is the cost to process queries, including SQL queries, user-defined functions, scripts, 
  and certain data manipulation language (DML) and data definition language (DDL) statements.
  - Queries (on-demand) **$6.25 per TB, the first 1 TB per month is free**
- **Storage** - is the cost to store data that you load into BigQuery
  - **$0.02 per GB per month**
  - After 90 days with no edits, price drops to $0.01 per GB per month
  - No charges for reading data from storage

**Cached or non-cached query results**
- If no destination table specified, query results are cached in **Temporary** table
- Temporary table stays in cache for one day
- If you run a query again within 24 hours, there is no charge
- If you run a query again and specify a destination table to store results, it won't read from cache, and you will be charge.

### Run Query from Python

#### Install Python Client for Google BigQuery
You can install BigQuery Python package by one of options below:
##### 1. Install by PIP
  Open a terminal, run the command:
  ```shell
   pip install google-cloud-bigquery
  ```
  
##### 2. Install by PyCharm
- Select **View -> Tool Windows -> Python Packages**
- Type **bigquery** in search field
- Select **google-cloud-bigquery** in the list
- Click **Install package**


  ![bq15](../../image/bq15.PNG)

#### Python Script
1. Copy following Python code and save to the file **sample_bq_query.py**
```shell
from google.cloud import bigquery
from google.oauth2 import service_account

## construct credentials from service account key file
credentials = service_account.Credentials.from_service_account_file(
    'agu55-big-data-fall2023_srvacct.json')  ## relative file path
## '/home/PGU6/workspace/big-data-class/gcp/bq_srv_acct.json') ## absolute file path

## construct a BigQuery client object
client = bigquery.Client(credentials=credentials)

## Your logics implementation goes below
# Perform a query.
QUERY = (
    'SELECT name, gender FROM `bigquery-public-data.usa_names.usa_1910_2013` '
    'WHERE state = "TX" '
    'LIMIT 5')
query_job = client.query(QUERY)  # API request
rows = query_job.result()  # Waits for query to finish

for row in rows:
    print("By index:  name=" + row[0] + ", gender=" + row[1])  ## by index
    print("By column: name=" + row.name + ", gender=" + row.gender)  ## by column name



```
2. Run Python Script
```shell
py sample_bq_query.py

or 
python sample_bq_query.py
```

Outputs
```shell
By index:  name=Ruby, gender=F  
By column: name=Ruby, gender=F  
By index:  name=Louise, gender=F
By column: name=Louise, gender=F
By index:  name=Carrie, gender=F
By column: name=Carrie, gender=F
By index:  name=Lucy, gender=F
By column: name=Lucy, gender=F
By index:  name=Elizabeth, gender=F
By column: name=Elizabeth, gender=F

```

## Good Practices

<img src="../../image/bq21.PNG" style="width: 95%; height: 95%"/>
<img src="../../image/bq23.PNG" style="width: 95%; height: 95%"/>
<img src="../../image/bq24.PNG" style="width: 95%; height: 95%"/>
<img src="../../image/bq25.PNG" style="width: 95%; height: 95%"/>
<img src="../../image/bq26.PNG" style="width: 95%; height: 95%"/>
<img src="../../image/bq27.PNG" style="width: 95%; height: 95%"/>
<img src="../../image/bq28.PNG" style="width: 95%; height: 95%"/>
<img src="../../image/bq29.PNG" style="width: 95%; height: 95%"/>

## References
#### [Introduction to SQL in BigQuery](https://cloud.google.com/bigquery/docs/introduction-sql#sql)
An overview of supported statements and SQL dialects in BigQuery.
#### [BigQuery Query Syntax](https://cloud.google.com/bigquery/docs/reference/standard-sql/query-syntax)
The syntax for SQL queries in GoogleSQL for BigQuery.
#### [Get Table Properties by Python](https://cloud.google.com/bigquery/docs/samples/bigquery-get-table#bigquery_get_table-python)
How to retrieve the properties of a table for a given table ID using Python
#### [BigQuery Query Optimization & Best Practices](https://cloud.google.com/bigquery/docs/best-practices-performance-compute)
Best practices for optimizing your query performance.

from google.cloud import bigquery
from google.oauth2 import service_account

## construct credentials from service account key file
credentials = service_account.Credentials.from_service_account_file(
    './.ssl/pgu6-msba2024-emory_srvacct.json') ## relative file path
## '/mnt/c/Users/PGU6/workspace-GBS/student-technology-tools/docs/gcp/.ssl/bq_srv_acct.json') ## absolute file path

## construct a BigQuery client object
client = bigquery.Client(credentials=credentials)

## Your logics implementation goes below
# Perform a query.
QUERY = (
    'SELECT name FROM `bigquery-public-data.usa_names.usa_1910_2013` '
    'WHERE state = "TX" '
    'LIMIT 5')
query_job = client.query(QUERY)  # API request
rows = query_job.result()  # Waits for query to finish

for row in rows:
    print(row[0]) ## by index
    print(row.name) ## by column name
