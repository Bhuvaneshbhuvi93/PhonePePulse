# PhonePe Pulse Dashboard
In this dashboard we are using the data that we extrascted from the state-wise json files in the repository which are uploaded into MySQL database. Then using the data dowmloded from SQL database the geo visualization is created.

## Step-1 *Importting Libraries*

Before proceedind further importing the necessary libraries in important. In this dash board we use streamlit, plotly to visualize our data. 

```
import streamlit as st
import mysql.connector
import pandas as pd
import json
import plotly.express as px
import requests
from streamlit_lottie import st_lottie
from streamlit_option_menu import option_menu
```
## Step-2 *Connecting to Database*

Next we coonect to the database were we stored our six dataframes which are transformed in a way to best suit for the visualization

```
config = {
  'user': '',
  'password': '',
  'host': '127.0.0.1',
  'database': '',
  'raise_on_warnings': True
}

# Connect to the database
cnx = mysql.connector.connect(**config)

# Check if the connection is successful
if cnx.is_connected():
  print("Connection to MySQL database established.")
else:
  print("Connection to MySQL database failed.")
```
## Step-3 *Downloading the tables*
Here we dowmload the dataframes cnx.cursor() funtion.

```
# Create a cursor to execute SQL queries
cursor = cnx.cursor()

# Define the SQL query to retrieve data from the "Top_India_2022_District" table
query = "SELECT * FROM aggregate_transaction"

# Execute the SQL query and store the result in a Pandas dataframe
aggregate_transaction = pd.read_sql(query, cnx)

# Print the first 5 rows of the dataframe
print(aggregate_transaction.head())
```

## Step-4 *Matching the state name*
Creating id which is used for the Geo visualization, here I used this [geojson file]("https://gist.githubusercontent.com/jbrobst/56c13bbbf9d97d187fea01ca62ea5112/raw/e388c4cae20aa53cb5090210a42ebb9b765c0a36/india_states.geojson",) to visualise the data. 
In the geojson file the state names are mentioned and their co-ordinated is given. In order to correctly plot out data in INDIA map we need to match the statename in gejson file to out state names in our state colum in every table

```
aggregate_transaction['state'] = aggregate_transaction['state'].replace({'andaman-&-nicobar-islands': 'Andaman & Nicobar','andhra-pradesh':'Andhra Pradesh', 'arunachal-pradesh':'Arunachal Pradesh',
       'assam':'Assam', 'bihar':'Bihar', 'chandigarh':'Chandigarh', 'chhattisgarh':'Chhattisgarh',
       'dadra-&-nagar-haveli-&-daman-&-diu':'Dadra and Nagar Haveli and Daman and Diu', 'delhi': 'Delhi', 'goa':'Goa', 'gujarat': 'Gujarat',
       'haryana':'Haryana','himachal-pradesh':'Himachal Pradesh', 'jammu-&-kashmir':'Jammu & Kashmir', 'jharkhand':'Jharkhand',
       'karnataka':'Karnataka', 'kerala':'Kerala', 'ladakh':'Ladakh', 'lakshadweep':'Lakshadweep', 'madhya-pradesh':'Madhya Pradesh',
       'maharashtra':'Maharashtra', 'manipur':'Manipur', 'meghalaya':'Meghalaya', 'mizoram':'Mizoram', 'nagaland':'Nagaland',
       'odisha':'Odisha', 'puducherry':'Puducherry', 'punjab':'Punjab', 'rajasthan':'Rajasthan', 'sikkim':'Sikkim',
       'tamil-nadu': 'Tamil Nadu', 'telangana':'Telangana', 'tripura':'Tripura', 'uttar-pradesh':'Uttar Pradesh',
       'uttarakhand':'Uttarakhand', 'west-bengal':'West Bengal'})
```

## Step-5 *Declaring Functions*
Once we match the state names it is ready to to be ploted in INDIA map. before plotting we need to filter through the data and then visualise the filtered data.

like below I have created various function based on two argumnets year and quarter. 

```
def transaction_agg(year,quarter):
    return aggregate_transaction[(aggregate_transaction['year'] == year) & (aggregate_transaction['quarter'] == quarter)]
```
## Step-6 *Streamlit Programe*

In here use your own imagination and create your dashboard using stremlit, If you look at my dashbord function block it has various charts and dataframe displayed using plotly graph. likewise you could also try to visualize in more beeter and efficient way.

## Conclusion

The final product of this project is the live geo visualization dashboard that displays
information and insights from the Phonepe pulse Github repository in an interactive
and visually appealing manner.
