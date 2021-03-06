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

## 4) Assignment activity 4
 ### OBJECTIVE 1
 # 'Difference per region' shows the number partially vaccinated people, i.e. individuals with the First Dose only.
 covid_vaccination = pd.DataFrame()
 covid_vaccination ['First Dose'] = covid_results.groupby('Province/State')['First Dose'].agg('sum')
 covid_vaccination ['Second Dose'] = covid_results.groupby('Province/State')['Second Dose'].agg('sum')
 covid_vaccination ['Difference per region'] = covid_vaccination ['First Dose'] - covid_vaccination ['Second Dose']
 covid_vaccination
 # 'Ratio of Intererest' shows the relative number of partially vaccinated people, i.e. the percentage difference between the individuals eligible for the Second Dose and individuals with the First Dose only.
 covid_vaccination ['Ratio of Intererest'] = covid_vaccination ['Difference per region'] / covid_vaccination ['First Dose'] * 100
 covid_vaccination
 # The Rate of Interest = 4.5 shows that total number of people eligible for the Second Dose [Difference per region] constitute only 4.5% of the total number of partially vaccinated people.
 # First Percentage
 # Absolute number of fully vaccinated population, i.e. individuals with First and Second dose
 covid_vaccination ['Fully-dosed'] = covid_vaccination ['First Dose'] + covid_vaccination ['Second Dose']
 # 'First Percentage' shows the ratio of individuals with First Dose to fully-dosed population.
 covid_vaccination ['First Percentage'] = covid_vaccination ['First Dose'] / covid_vaccination ['Fully-dosed'] * 100
 covid_vaccination
 covid_vaccination.reset_index(inplace=True)
 # Plot a vertical bar graph to display the percentage of the first dose to fully-dosed individuals.
 ax = sns.barplot(x='Province/State', y='First Percentage', data=covid_vaccination, color = 'steelblue') 
 # Add distinct tick labels to the barplot.
 ax.set_title("Percentage of Individuals with First Dose to Fully-dosed Population", fontsize = 16, fontweight = 'bold')
 ax.set_ylabel("First Percentage(%)", fontsize = 14)
 ax.set_xlabel("Province/State", fontsize = 14)
 plt.xticks(rotation=30, ha='right', fontsize = 14)
 plt.tight_layout()
 plt.show()
 fig = ax.get_figure()
 fig.savefig('covid_vax_chart1.png')
 # Plot a chart to display the total number of individuals who have received their First Dose.
 ax = sns.barplot(x='Province/State', y='First Dose', data=covid_vaccination, color = 'steelblue') 
 ax.set_title("Number of Individuals with First Dose", fontsize = 16, fontweight = 'bold')
 ax.set_ylabel("First Dose", fontsize = 14)
 ax.set_xlabel("Province/State", fontsize = 14)
 plt.xticks(rotation=30, ha='right', fontsize = 14)
 plt.tight_layout()
 plt.show()
 fig = ax.get_figure()
 fig.savefig('first_dose.png')
 # Plot a chart to display the total number of individuals who have received their Second Dose.
 ax = sns.barplot(x='Province/State', y='Second Dose', data=covid_vaccination, color = 'steelblue')
 ax.set_title("Number of Individuals with Second Dose", fontsize = 16, fontweight = 'bold')
 ax.set_ylabel("Second Dose", fontsize = 14)
 ax.set_xlabel("Province/State", fontsize = 14)
 plt.xticks(rotation=30, ha='right', fontsize = 14)
 plt.tight_layout()
 plt.show()
 fig = ax.get_figure()
 fig.savefig('second_dose.png')
 # A grouped bar chart with a side-by-side comparison of First Dose and Second Dose for the UK Government
 First_Dose = [4931470, 2817981, 5166303, 3522476, 3287646, 3757307, 5870786, 4226984, 5401128, 2583151, 2348310, 3052822]
 Second_Dose = [4709072, 2690908, 4933315, 3363624, 3139385, 3587869, 5606041, 4036345, 5157560, 2466669, 2242421, 2915136]
 index = ['Anguilla', 'Bermuda', 'British Virgin Islands', 'Cayman Islands', 'Channel Islands', 'Falkland Islands (Malvinas)',
          'Gibraltar', 'Isle of Man', 'Montserrat', 'Others', 'Saint Helena, Ascension and Tristan da Cunha', 'Turks and Caicos Islands']
 gov_chart = pd.DataFrame({'First Dose': First_Dose,
                           'Second Dose': Second_Dose}, index=index)

 sns.set_palette("Paired")
 ax = gov_chart.plot.bar()
 plt.title("First Dose and Second Dose Comparison", fontsize = 16, fontweight = 'bold')
 plt.xlabel("Province/State", fontsize = 14)
 plt.ylabel("Count (hundred thousand)", fontsize = 14)
 plt.xticks(rotation=30, ha='right', fontsize = 14)
 plt.tight_layout()
 plt.show()
 fig = ax.get_figure()
 fig.savefig('gov_chart.png')
 ### OBJECTIVE 2
 # Group the data by 'Province/State' and 'Date', and aggregate 'Deaths'
 covid_vaccination = pd.DataFrame()
 covid_vaccination ['Deaths'] = covid_results.groupby(['Province/State', 'Date'])['Deaths'].agg('sum')
 covid_vaccination.tail()
 # Visualise daily change in deaths by Province/State
 covid_vaccination.reset_index(inplace=True)
 ax = sns.barplot(x='Province/State',y='Deaths', data=covid_vaccination, color = 'steelblue')
 sns.set(rc = {'figure.figsize':(15,8)})
 plt.title("Daily Number of Deaths by Province/State", fontsize = 16, fontweight = 'bold')
 ax.set_ylabel("Deaths", fontsize = 14)
 ax.set_xlabel("Province/State", fontsize = 14)
 plt.xticks(rotation=30, ha='right', fontsize = 14)
 plt.tight_layout()
 plt.show()
 fig = ax.get_figure()
 fig.savefig('covid_vax_chart2.png')
 # Visualise daily change in deaths across Province/State over time, including values data for 'Others'
 ax = sns.lineplot(x='Date',y='Deaths', data=covid_vaccination, color = 'steelblue')
 sns.set(rc = {'figure.figsize':(15,8)}) 
 plt.title("Daily Number of Deaths with Others Over Time", fontsize = 16, fontweight = 'bold')
 ax.set_ylabel("Deaths", fontsize = 14)
 ax.set_xlabel("Date", fontsize = 14)
 plt.xticks(rotation=30, ha='right', fontsize = 0)
 plt.tight_layout()
 plt.show()
 fig = ax.get_figure()
 fig.savefig('chart2_over_time.png')
 # Group the data by 'Province/State' and aggregate 'Deaths'.
 # Group the data by 'Province/State' and aggregate 'Deaths'.
 covid_vaccination = pd.DataFrame()
 covid_vaccination ['Deaths'] = covid_results.groupby(['Province/State'])['Deaths'].agg('sum')
 covid_vaccination
 # --> The aggregated death count for "Others" causes the data set to be skewed.
 # Exclude Province/State causing the skewed data set.
 covid_vaccination.reset_index(inplace=True)
 covid_vac_without_Others = covid_vaccination[covid_vaccination['Province/State'] != 'Others']
 covid_vac_without_Others
 # Re-create the lineplot excluding the "Others" that is causing the skewed data set.
 ax = sns.barplot(x='Province/State',y='Deaths', data=covid_vac_without_Others, color = 'steelblue')
 sns.set(rc = {'figure.figsize':(15,8)})
 plt.title("Daily Number of Deaths by Province/State without Others", fontsize = 16, fontweight = 'bold')
 ax.set_ylabel("Deaths", fontsize = 14)
 ax.set_xlabel("Province/State", fontsize = 14)
 plt.xticks(rotation=30, ha='right', fontsize = 14)
 plt.tight_layout()
 plt.show()
 fig = ax.get_figure()
 fig.savefig('covid_vax_chart3.png')
 # Group the data by 'Province/State' and 'Date', and aggregate 'Deaths'
 covid_vaccination = pd.DataFrame()
 covid_vaccination ['Deaths'] = covid_results.groupby(['Province/State', 'Date'])['Deaths'].agg('sum')
 covid_vaccination
 # Sort the data by 'Province/State' and 'Date'
 covid_vaccination.sort_values(by="Date")
 # Remove 'Others' that previously cause skewed data from the DataFrame sorted by 'Province/State' and 'Date'.
 covid_vaccination.reset_index(inplace=True)
 covid_vac_without_Others = covid_vaccination[covid_vaccination['Province/State'] != 'Others']
 covid_vac_without_Others
 # Visualise DAILY change in deaths across Province/State over time excluding 'Others'
 ax = sns.lineplot(x='Date',y='Deaths', data=covid_vac_without_Others, color = 'steelblue')
 sns.set(rc = {'figure.figsize':(15,8)}) 
 plt.title("Daily Number of Deaths without Others Over Time", fontsize = 16, fontweight = 'bold')
 ax.set_ylabel("Deaths", fontsize = 14)
 ax.set_xlabel("Date", fontsize = 14)
 plt.xticks(rotation=30, ha='right', fontsize = 0)
 plt.tight_layout()
 plt.show()
 fig = ax.get_figure()
 fig.savefig('chart3_over_time.png')
 # Smooth out the data by looking at monthly figures.
 # Convert Date into Months and plot the same line graph as previously.
 covid_vac_without_Others['Month'] = pd.to_datetime(covid_vac_without_Others['Date']) + pd.offsets.MonthBegin(0)
 covid_vac_without_Others
 # Convert Date into Months and plot the same line graph as previously. 
 ax = sns.barplot(x='Province/State',y='Deaths', data=covid_vac_without_Others, color = 'steelblue')
 sns.set(rc = {'figure.figsize':(15,8)}) 
 plt.title("Monthly Number of Deaths by Province/State without Others", fontsize = 16, fontweight = 'bold')
 ax.set_ylabel("Deaths", fontsize = 14)
 ax.set_xlabel("Province/State", fontsize = 14)
 plt.xticks(rotation=30, ha='right', fontsize = 14)
 plt.tight_layout()
 plt.show()
 fig = ax.get_figure()
 fig.savefig('covid_vax_chart4.png')
 # Visualise MONTHLY change in deaths across Province/State over time  excluding 'Others'
 ax = sns.lineplot(x='Month',y='Deaths', data=covid_vac_without_Others, color = 'steelblue')
 sns.set(rc = {'figure.figsize':(15,8)}) 
 plt.title("Monthly Number of Deaths without Others Over Time", fontsize = 16, fontweight = 'bold')
 ax.set_ylabel("Deaths", fontsize = 14)
 ax.set_xlabel("Month", fontsize = 14)
 plt.xticks(fontsize = 14)
 plt.tight_layout()
 plt.show()
 fig = ax.get_figure()
 fig.savefig('chart4_over_time.png')
 ### OBJECTIVE 3
 # Group the data by 'Province/State' and ggregate the count of 'Recovered' cases.
 # Sort the values of recovered cases in ascending order.
 covid_vaccination = pd.DataFrame()
 covid_vaccination ['Recovered'] = covid_results.groupby('Province/State')['Recovered'].agg('sum').sort_values(ascending=True)
 covid_vaccination
 # Group the data by 'Province/State' and 'Date', and ggregate the count of 'Recovered' cases.
 covid_vaccination = pd.DataFrame()
 covid_vaccination ['Recovered'] = covid_results.groupby(['Province/State', 'Date'])['Recovered'].agg('sum').sort_values(ascending=True)
 covid_vaccination
 # Smooth out the data by looking at monthly figures.
 # Convert Date into Months.
 covid_vaccination.reset_index(inplace=True)
 covid_vaccination['Month'] = pd.to_datetime(covid_vaccination['Date']) + pd.offsets.MonthBegin(0)
 # Create a lineplot to visualise the trend of recovered cases across the months.
 ax = sns.lineplot(x='Month',y='Recovered', data=covid_vaccination, color = 'steelblue')
 sns.set(rc = {'figure.figsize':(15,8)}) 
 plt.title("Monthly Number of Recovered Cases", fontsize = 16, fontweight = 'bold')
 ax.set_ylabel("Recovered Individuals", fontsize = 14)
 ax.set_xlabel("Month", fontsize = 14)
 plt.xticks(rotation=30, ha='right', fontsize = 14)
 plt.tight_layout()
 plt.show()
 fig = ax.get_figure()
 fig.savefig('chart5_over_time.png')
 # Visualise monthly change in recovered cases by Province/State. 
 ax = sns.barplot(x='Province/State',y='Recovered', data=covid_vaccination, color = 'steelblue')
 sns.set(rc = {'figure.figsize':(15,8)}) 
 plt.title("Monthly Number of Recovered Cases by Province/State", fontsize = 16, fontweight = 'bold')
 ax.set_ylabel("Recovered Individuals", fontsize = 14)
 ax.set_xlabel("Month", fontsize = 14)
 plt.xticks(rotation=30, ha='right', fontsize = 14)
 plt.tight_layout()
 plt.show()
 fig = ax.get_figure()
 fig.savefig('covid_vax_chart5.png')
 
 # Difference% between first and second dose by province/region shows the percentage of partially vaccinated people overtime, i.e. the ratio of partially vaccinated to fully vaccinated
 covid_vaccination ['Difference%'] = covid_vaccination ['Difference per region'] / covid_vaccination ['Second Dose'] * 100
 covid_vaccination

 ## 5) Assignment activity 5: External data 
 import pandas as pd
 import seaborn as sns
 pd.options.display.max_colwidth = 200
 sns.set(rc = {'figure.figsize':(15,10)})
 # Import the tweet data set
 tweets = pd.read_csv('tweets.csv')
 # Explore the data: info(), head()
 tweets.info()
 tweets.head()
 # Explore the structure, count the tweets, get the elements of interest
 tweets.retweet_count.value_counts()
 tweets.favorite_count.value_counts()
 # Create a DataFrame with the text only
 tweets['text'] = tweets['text'].astype(str)
 tweets_text = tweets['text'].apply(lambda x: x if x.strip() != None else None)
 ### a. Identify the hashtags.
 # Loop through the messages and build a list of values containing the #-symbol
 tags = []
 for y in [x.split(' ') for x in tweets_text.values]:
     for z in y:
         if '#' in z:
             tags.append(z)
 # Create a Series that counts the values in the list of values containing the #-symbol.
 tags = pd.Series(tags).value_counts()
 # Filter and sort
 # Display the first 30 records.
 tags.head(30)
 ### b. Identify the top trending hashtags
 # Convert Series into a DataFrame
 data = pd.DataFrame(tags).reset_index()
 # Name the columns in the newly created Pandas DataFrame 
 data.columns = ['word', 'count']
 # Fix the data types in the newly created Pandas DataFrame 
 data['count'] = data['count'].astype(int)
 data
 # Display the records for the top trending hashtags with the count larger than 100
 display(data.loc[(data['count']>100)])
 # Plot
 # Display the data in a visualisation and identify the top trending hashtags related to COVID. 
 ax = sns.barplot(x="count",y="word", data=data.loc[(data['count']>100)], color = 'steelblue')
 fig = ax.get_figure()
 fig.savefig('twitter_hashtags.png')
 
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
