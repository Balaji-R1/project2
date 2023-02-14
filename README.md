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
##### Clean the data and handle missing values :
     df = df.dropna()
     df = df.drop_duplicates()
##### Transform the data into a format suitable for analysis and visualization:
     df["date"] = pd.to_datetime(df["date"])
     df["total_transactions"] = df["success_transactions"] + df["failure_transactions"]

### 
