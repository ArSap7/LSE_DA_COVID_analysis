# LSE_DA_COVID_analysis
Analysis of the COVID-19 statistics to inform the UK government vaccination programme.
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
