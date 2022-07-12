# LSE_DA_COVID_analysis
Analysis of the COVID-19 statistics to inform the UK government vaccination programme.
## 0) Environment preparation
# Import the required libraries and set the plotting options
import numpy as np
import pandas as pd
import seaborn as sns
import matplotlib.pyplot as plt
sns.set(rc = {'figure.figsize':(15,10)})
## 2) Assignment activity 1
[My Github Repo](https://github.com/ArSap7/LSE_DA_COVID_analysis)
['My Github screenshot](https://github.com/ArSap7/LSE_DA_COVID_analysis/blob/main/GitHubScreenshot.png?raw=true)
## 2) Assignment activity 2
# Load the COVID-19 cases and vaccine data sets as cov and vac respectively
pd.read_csv('covid_19_uk_cases.csv')
# Load the COVID-19 cases and vaccine data sets as cov and vac respectively
pd.read_csv('covid_19_uk_vaccinated.csv')
cov = pd.read_csv('covid_19_uk_cases.csv')
vac = pd.read_csv('covid_19_uk_vaccinated.csv')
# Explore the DataFrames with the appropriate functions
# View the first five rows for 'covid_19_uk_cases.csv'.
cov.head()
# View the last five rows for 'covid_19_uk_cases.csv'.
cov.tail()
# View the first five rows for 'covid_19_uk_vaccinated.csv'.
vac.head()
# View the last five rows for 'covid_19_uk_vaccinated.csv'.
vac.tail()
# Determine the data types in the DataFrames for 'covid_19_uk_cases.csv'.
cov.info()
# Determine the data types in the DataFrames for 'covid_19_uk_vaccinated.csv'.
vac.info()
# Determine the number of missing values for 'covid_19_uk_cases.csv'.
cov_na = cov[cov.isna().any(axis=1)]
cov_na.shape
# There are 2 rows with NaN values in 12 columns of the cov DataFrame.
cov_na = cov[cov.isna().any(axis=1)]
display(cov_na)
# Determine the number of missing values for 'covid_19_uk_vaccinated.csv'.
vac_na = vac[vac.isna().any(axis=1)]
vac_na.shape
# There are 0 rows with NaN values in 11 columns of the vac DataFrame. 
# Determine the number of rows and columns for 'covid_19_uk_cases.csv'.
cov.shape
# Determine the number of missing values for 'covid_19_uk_vaccinated.csv'.
vac.shape
# Filter the data for the region Gibraltar as displayed in the 'covid_19_uk_cases.csv'. (Hint: You can use the df[df[col]==index] function.)
cov[cov['Province/State']=='Gibraltar']
# Create DataFrame based on Gibraltar data
# Hint: newdf = df[df[col]==index]
cov_Gibraltar = cov[cov['Province/State']=='Gibraltar']
#print the whole DataFrame
#replace df with the name you have given your DataFrame
pd.set_option("display.max_rows", None)
cov_Gibraltar 
# Explore behaviour over time
#Subset the Gibraltar DataFrame that you have created consisting of the following columns: Deaths, Cases, Recovered and Hospitalised.
# Select few columns.
cov_Gibraltar_fcol = pd.read_csv('covid_19_uk_cases.csv', 
                            usecols=['Deaths', 'Cases', 'Recovered', 'Hospitalised'])
# Print the DataFrame.
cov_Gibraltar_fcol
#Run the describe() function to generate descriptive statistics.
cov_Gibraltar_fcol.describe()
