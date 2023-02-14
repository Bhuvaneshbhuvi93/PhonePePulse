# PhonePe Pulse
## About PhonePePulse

The Indian digital payments story has truly captured the world's imagination. From the largest towns to the remotest villages, there is a payments revolution being driven by the penetration of mobile phones and data.

When PhonePe started 5 years back, we were constantly looking for definitive data sources on digital payments in India. Some of the questions we were seeking answers to were - How are consumers truly using digital payments? What are the top cases? Are kiranas across Tier 2 and 3 getting a facelift with the penetration of QR codes?
This year as we became India's largest digital payments platform with 46% UPI market share, we decided to demystify the what, why and how of digital payments in India.

This year, as we crossed 2000 Cr. transactions and 30 Crore registered users, we thought as India's largest digital payments platform with 46% UPI market share, we have a ring-side view of how India sends, spends, manages and grows its money. So it was time to demystify and share the what, why and how of digital payments in India.

*PhonePe Pulse* is your window to the world of how India transacts with interesting trends, deep insights and in-depth analysis based on our data put together by the PhonePe team.

## Data Cloning

The first step to create a dashboard is to clone the phonepe pulse repository in the local system and that could be done by below code.

```
#cloning the Github repository using url
repo_url = "https://github.com/PhonePe/pulse.git"
#cloning the repository to below directory
repo_dir = "pulse"

# checking if the directory exists already
if not os.path.exists(repo_dir):
    subprocess.call(["git", "clone", repo_url, repo_dir]) 
```

## Importing Libraries

```
import os
import subprocess
import json
import pandas as pd
# using sqlalchemy
import mysql.connector
from sqlalchemy import create_engine
import datetime as dt 
import glob
```
## Understanding the data

Before we process the dataset, A good understanding of the data is needed for that we need to know how the data is available in the repository.
a clear expalanation of the data is given in below link of **PhonePe Pulse** repository

Here is the link [*PhonePe Pulse*](https://github.com/PhonePe/pulse.git)

## Creating the Dataframes

Once we understand the data we could proceed to extract the data from json file provided to us. We need to create totaly a 6 dataframes based on the data given to us.
for every dataframe created we need to use glob function to automate the searching of the folders and them create a for loop to get out required data then store them in a data frame. To create the 6 dataframes i have used only the state data and ignored the india data beacause using state data we could easily bring the india data by adding all up
1.Aggregate_transaction
2.Aggregate_user
3.map_transaction
4.map_user
5.top_transaction
6.top_user

## Glob function

In Python, the glob module is used to find all the pathnames matching a specified pattern according to the rules used by the Unix shell, although results are returned in arbitrary order. The glob function returns a list of file paths that match the specified pattern.

The pattern you provided is:

```
F:\Simple_projects\Capstone_PhonePe_Plus\pulse\data\aggregated\transaction\*\*\state\*\*\*.json
```

This pattern is looking for all the JSON files within a directory structure that has the following pattern:

```
F:\Simple_projects\Capstone_PhonePe_Plus\pulse\data\aggregated\transaction\<any folder>\<any folder>\state\<any folder>\<any folder>\*.json

```
For example, this pattern would match file paths such as:

```
F:\Simple_projects\Capstone_PhonePe_Plus\pulse\data\aggregated\transaction\folder1\folder2\state\folder3\folder4\file1.json
F:\Simple_projects\Capstone_PhonePe_Plus\pulse\data\aggregated\transaction\folder5\folder6\state\folder7\folder8\file2.json
```
## Aggregate_transaction
the code to create dataframe of **Aggregate_transaction** with unique columns of state, year, quarter. for further clarification see below code.

```
# Declaring the path of the files using glob function
path = 'F:\\Simple_projects\\Capstone_PhonePe_Plus\\pulse\\data\\aggregated\\transaction\\*\\*\\state\\*\\*\\*.json'
files = glob.glob(path)
```

```
# in this code block I am creating a for loop to fetch data from the path which has state names, year and also the quarter number in it, Using this we could easily create a three unique column for the dataframe
dfs = []

for file in files: 
    parts = file.split("\\")
    state_name = parts[10]
    year = int(parts[11][:4])
    quarter_number = int(parts[12][0])
    quarter = "Q" + str(quarter_number)
    
    with open(file) as f:
        data = json.load(f)
    
    transactions = []
    for transaction in data['data']['transactionData']:
        name = transaction['name']
        payment_instrument = transaction['paymentInstruments'][0]
        count = int(payment_instrument['count'])
        amount = int(payment_instrument['amount'])
        transactions.append({'country': 'India','state': state_name,'year': year, 'quarter': quarter, 'transaction_type': name, 'transaction_count': count, 'total_amount': amount})
    
    df = pd.DataFrame(transactions)
    dfs.append(df)

df_concat_agg_tran = pd.concat(dfs)
df_concat_agg_tran = df_concat_agg_tran.reset_index(drop=True)

```
once you run this code the dataframe will look like this with datas from every states.

```
country	state	year	quarter	transaction_type	transaction_count	total_amount
0	India	andaman-&-nicobar-islands	2018	Q1	Recharge & bill payments	4200	1845307
1	India	andaman-&-nicobar-islands	2018	Q1	Peer-to-peer payments	1871	12138655
2	India	andaman-&-nicobar-islands	2018	Q1	Merchant payments	298	452507
3	India	andaman-&-nicobar-islands	2018	Q1	Financial Services	33	10601
4	India	andaman-&-nicobar-islands	2018	Q1	Others	256	184689
...	...	...	...	...	...	...	...
3589	India	west-bengal	2022	Q4	Peer-to-peer payments	184380244	620222191587
3590	India	west-bengal	2022	Q4	Merchant payments	171667404	140807748126
3591	India	west-bengal	2022	Q4	Recharge & bill payments	48921147	26026633153
3592	India	west-bengal	2022	Q4	Financial Services	268388	261122865
3593	India	west-bengal	2022	Q4	Others	610414	457937874
```

like this you could create all six dataframes and upload them into MySQL database.

## SQL Connector

once the dataframes are created, we should use my sql connecter to upload the data using below code

```
# using create_engine module opening the MySql with correct credentials
engine = create_engine('mysql+mysqlconnector://username:password@127.0.0.1:3306/databasename')
```
