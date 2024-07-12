#Task 5


import time
import pandas as pd
from sqlalchemy import create_engine

# Define your database connection parameters
source_db_connection_string = 'mssql+pyodbc://username:password@source_server/database?driver=ODBC+Driver+17+for+SQL+Server'

# Create a database engine
source_engine = create_engine(source_db_connection_string)

# Function to retrieve data from the source database
def retrieve_data(query):
    with source_engine.connect() as connection:
        return pd.read_sql(query, connection)

# Function to simulate cut or copy (just a placeholder)
def cut_or_copy_data(dataframe):
    # For demonstration purposes, we'll just print the data
    print("Data copied or cut:")
    print(dataframe)

# Define your SQL query to retrieve data
sql_query = "SELECT * FROM your_table WHERE last_modified_date > '2023-01-01'"

# Retrieve data
data = retrieve_data(sql_query)

# Wait for a few seconds
time.sleep(5)

# Cut or copy data
cut_or_copy_data(data)
