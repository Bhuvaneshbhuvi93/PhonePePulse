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



