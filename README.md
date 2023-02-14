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

