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

## 3) Assignment activity 3
# Join the DataFrames as covid where you merge cov and vac
# Merge the DataFrames
covid = pd.merge(cov, vac)
# View the DataFrame
covid
# Explore the new DataFrame
# Determine the column names and data types.
covid.info()
# Fix the date column data type 
# Convert the data type of the Date column from object to datetime.
covid['Date'] = pd.to_datetime(covid['Date'])
covid.info()
# Clean up / drop unnecessary columns
covid_results = covid.drop(['Lat', 'Long', 'ISO 3166-1 Alpha 3-Codes', 'Sub-region Name', 'Intermediate Region Code'], axis = 1)
covid_results
# Groupby and calculate difference between first and second dose
# First dose total by province/state
covid_vaccination = pd.DataFrame()
# First Dose total
covid_vaccination ['First Dose'] = covid_results.groupby('Province/State')['First Dose'].agg('sum')
# Second dose total
covid_vaccination ['Second Dose'] = covid_results.groupby('Province/State')['Second Dose'].agg('sum')
covid_vaccination
# Difference between first and second dose shows the total number of partially vaccinated people by province/state, i.e. people with the First Dose only
covid_vaccination ['Difference per region'] = covid_vaccination ['First Dose'] - covid_vaccination ['Second Dose']
covid_vaccination
# Difference% between first and second dose shows the percenrage of partially vaccinated people by province/region, i.e. the ratio of partially vaccinated to fully vaccinated
covid_vaccination ['Difference%'] = covid_vaccination ['Difference per region'] / covid_vaccination ['Second Dose'] * 100
covid_vaccination
# Groupby and calculate the difference between first and second dose over time
covid_vaccination = pd.DataFrame()
# First Dose total
covid_vaccination ['First Dose'] = covid_results.groupby(['Province/State', 'Date'])['First Dose'].agg('sum')
# Second Dose total
covid_vaccination ['Second Dose'] = covid_results.groupby(['Province/State', 'Date'])['Second Dose'].agg('sum')
# Difference between first and second dose by province/region shows the total number of partially vaccinated people overtime, i.e. people with the First Dose only
covid_vaccination ['Difference per region'] = covid_vaccination ['First Dose'] - covid_vaccination ['Second Dose']
covid_vaccination
# Difference% between first and second dose by province/region shows the percentage of partially vaccinated people overtime, i.e. the ratio of partially vaccinated to fully vaccinated
covid_vaccination ['Difference%'] = covid_vaccination ['Difference per region'] / covid_vaccination ['Second Dose'] * 100
covid_vaccination

## 6) Assignment activity 6: 
# Libraries and settings
import numpy as np
import pandas as pd
import matplotlib.pyplot as plt
%matplotlib inline
# Import additional libraries.
from sklearn.metrics import r2_score, median_absolute_error, mean_absolute_error
from sklearn.metrics import median_absolute_error, mean_squared_error, mean_squared_log_error
# Load and select relevant subset of the data
cov = pd.read_csv('covid_19_uk_cases.csv')
vac = pd.read_csv('covid_19_uk_vaccinated.csv')
sample = cov[['Province/State','Date','Hospitalised']]
# Select data for a specific province
sample_ci = sample[sample['Province/State'] == "Channel Islands"]
# Define function to plot moving averages
def plot_moving_average(series, window, plot_intervals=False, scale=1.96):
    
    # Create a rolling window to calculate the rolling mean using the series.rolling function
    rolling_mean = series.rolling(window=window).mean()
    
    # Declare the dimensions for the plot, plot name and plot the data consisting of the rolling mean from above 
    plt.figure(figsize=(18,4))
    plt.title('Moving average\n window size = {}'.format(window))
    plt.plot(rolling_mean, 'g', label='Simple moving average trend')

    
    # Plot confidence intervals for smoothed values
    if plot_intervals:
        
        # Calculate the mean absolute square 
        mae = mean_absolute_error(series[window:], rolling_mean[window:])
        
        # Calculate the standard deviation using numpy's std function
        deviation = np.std(series[window:] - rolling_mean[window:])
        
        # Calculate the upper and lower bounds 
        lower_bound = rolling_mean - (mae + scale * deviation)
        upper_bound = rolling_mean + (mae + scale * deviation)
        
        # Name and style upper and lower bound lines and labels 
        plt.plot(upper_bound, 'r--', label='Upper bound / Lower bound')
        plt.plot(lower_bound, 'r--')
    
    # Plot the actual values for the entire timeframe
    plt.plot(series[window:], label='Actual values')
    plt.grid(True)
# Define function to calculate the mean absolute error
def mean_absolute_error(a, b): return abs(b - a)
# Demonstrate the use of the function to plot moving averages.
# View the DataFrame.
print(sample_ci)
sample_ci.head()
# Plot the number of hospitalised individuals for the Channel Islands as a time series.
ax = sample_ci.plot(figsize=(12, 4), color = 'steelblue')
plt.legend(loc='best', fontsize = 14)
plt.title('Time Series Plot for Channel Islands', fontsize = 16, fontweight = 'bold')
plt.show(block=False)
fig = ax.get_figure()
fig.savefig('time_series_plot.png')
# Remove missing values.
sample_ci_na = sample_ci[sample_ci.isna().any(axis=1)]
sample_ci_na.shape
# Demonstrate the use of the function to plot moving averages
# 7-days smoothing
ax = plot_moving_average(sample_ci.Hospitalised, 7, plot_intervals=True)
# Return the top three days with biggest difference between daily value and rolling 7-day mean
s = sample_ci.copy()
s_rolling = s['Hospitalised'].rolling(window=7).mean()
s['error'] = mean_absolute_error(s['Hospitalised'][7:], s_rolling[7:])
s.sort_values('error', ascending=False).head(3)

