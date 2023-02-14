# Phonepe Pulse Data Visualization and Exploration:
  In this, we are going to visualize the trends and analysis done with online transactions data using GUI
##### Phonepe Pulse:
  * It is about how India transacts with interesting recent trends, deep insights and in-depth analysis based on our data put together by the PhonePe team.
  * It is launched by Phone for accurate and comprehensive data on digital payment transaction trends in India
  * PhonePe Pulse website showcases more than 2000+ Crore transactions by consumers on an interactive map of India.
  *  India's first interactive geospatial platform on digital payments
The transaction data is shared in Github repository for everyone to easily access for better understanding, insights and visualization on how digital payments have evolved over the years in India.

### Clone the pulse repository:
     import git
     repo = git.Repo.clone_from("https://github.com/phonepe/pulse.git", "local_folder")
 
### To manipulate and pre-process the data:
     import pandas as pd
     df = pd.read_csv("data.csv")
###### Clean the data and handle missing values :
     df = df.dropna()
     df = df.drop_duplicates()
###### Transform the data into a format suitable for analysis and visualization:
     df["date"] = pd.to_datetime(df["date"])
     df["total_transactions"] = df["success_transactions"] + df["failure_transactions"]

### To insert the transformed data into a MySQL database :
    import mysql.connector

###### Connect to the MySQL database
      cnx = mysql.connector.connect(user='username', password='password',
                                    host='localhost',
                                    database='phonepe_pulse')

###### Insert the transformed data using SQL commands
     cursor = cnx.cursor()
     query = "INSERT INTO transactions (date, success_transactions, failure_transactions, total_transactions) VALUES (%s, %s, %s, %s)"
     for index, row in df.iterrows():
         cursor.execute(query, (row["date"], row["success_transactions"], row["failure_transactions"], row["total_transactions"]))
     cnx.commit()
     cursor.close()
     cnx.close()
     
### Dashboard creation:
   import plotly.express as px
   import streamlit as st

##### Fetch data from the MySQL database into a Pandas dataframe
cnx = mysql.connector.connect(user='username', password='password',
                              host='localhost',
                              database='phonepe_pulse')
df = pd.read_sql_query("SELECT * FROM transactions", cnx)

##### Create a Plotly figure to display data on a map
fig = px.scatter_geo(df, lat="latitude", lon="longitude", color="total_transactions", size="total_transactions")

##### Create a Streamlit dashboard with dropdown options for users to select different facts and figures to display
st.title("Phonepe Pulse Dashboard")
options = ["Success Transactions", "Failure Transactions", "Total Transactions"]
selected_option = st.selectbox("Select an option", options)
if selected_option == "Success Transactions":
    st.write("Success Transactions:", df["success_transactions"].sum())
elif selected_option == "Failure Transactions":
    st.write("Failure Transactions:", df["failure_transactions"].sum())
else:
    st.plotly_chart(fig)

### To fetch the data from the MySQL database into a Pandas dataframe for dynamic updating of the dashboard:
##### Connect to the MySQL database and fetch the data into a Pandas dataframe
cnx = mysql.connector.connect(user='username', password='password',
                              host='localhost',
                              database='phonepe_pulse')
df = pd.read_sql_query("SELECT * FROM transactions", cnx)

