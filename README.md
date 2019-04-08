# Sorting-a-data-frame-and-plotting-a-time-series-to-find-which-country-is-richest-per-person.ipynb
### Using data from a .csv file, ranging from year 1952 onwards, the data frame has been sorted and visualised by plotting a time series ###  to find which country is the richest on a per person basis. I made created this using pandas, matplotlib and pyplot, run on Jupyter ###  Notebooks.

## For simplicity, countries' GDP per Capita will be compared.

import pandas as pd
from matplotlib import pyplot as plt

data = pd.read_csv('countries.csv')
data.head()

![01 Data head countries file](https://user-images.githubusercontent.com/48648985/55690816-315e4300-598e-11e9-8020-e4317be622d2.png)

### Finding the mean GDP per Capita for each country

data.groupby(['country']).mean()

### Group this data frame with the country column, and find the mean for 
###  all the other columns for each country.

![02 Grouped data on countries an mean GDP per Capita](https://user-images.githubusercontent.com/48648985/55724401-3b6e5900-5a03-11e9-8f44-d5f59ebf023d.png)

### Selecting the GDP per Capita data frame exclusively:

data.groupby(['country']).mean().gdpPerCapita

### These values are the GDP per Capita for all of the available years for 
###  each country.

![03 Grouped data GDP per Capita column](https://user-images.githubusercontent.com/48648985/55724678-d9622380-5a03-11e9-97b2-583e4190cd79.png)

mean_gdp_per_capita = data.groupby(['country']).mean().gdpPerCapita

top_5 = mean_gdp_per_capita.sort_values(ascending=False).head()
print(top_5)
### This will show the countries with the highest GDP per Capita first.

![04 Top 5 countries in terms of GDP per Capita](https://user-images.githubusercontent.com/48648985/55725099-d4ea3a80-5a04-11e9-8f43-e4ea1fc60b53.png)


## Is Kuwait really the richest country in the world on a per-person basis?

### Let us first isolate the data on Kuwait:

kuwait = data[data.country == 'Kuwait']
kuwait.head()

### Plotting how the GDP per Capita in Kuwait has changed over time with:

plt.plot(kuwait.year, kuwait.gdpPerCapita, color='darkgreen')
plt.title('GDP per Capita in Kuwait')
plt.show()

![05 GDP per Capita in Kuwait time series](https://user-images.githubusercontent.com/48648985/55725235-2abee280-5a05-11e9-995c-c56edbb5d4b0.png)


#### Interpreting the graph:
####  Between the 1950s and the early 1970s, Kuwait's GDP was very high, and 
####   over 80,000 USD, however it then dropped.

#### To visualise what happened with more detail:

plt.subplot(311)
plt.title('Kuwait\'s GDP per Capita')
plt.plot(kuwait.year, kuwait.gdpPerCapita, color='forestgreen')

plt.subplot(312)
plt.title('Kuwait\'s GDP in Billions $')
plt.plot(kuwait.year, kuwait.population*kuwait.gdpPerCapita / 10**9,
        color='black')

plt.subplot(313)
plt.title('Kuwait\'s Population in Millions')
plt.plot(kuwait.year, kuwait.population / 10**6, color='red')

plt.tight_layout() # This formats the layout correctly.
plt.show()

![06 3 subplots of Kuwait's Econometric time series information](https://user-images.githubusercontent.com/48648985/55725724-2d6e0780-5a06-11e9-820e-d228a6a60207.png)


#### Interpreting these graphs:
####  In the late 70s, GDP in Billions, dropped siginificantly, which can be 
####   attributed to a economic crisis at the time. The GDP eventually
####   increased in the 90s, but their population also grew, partly due to
####   high level of immigrants to the country.


## Comparing their relative growth in a single chart:

plt.plot(kuwait.year, kuwait.population / kuwait.population.iloc[0] * 100)

#### This way, the first year's population is set to 100% as the basis for 
####  comparison.

kuwait_gdp = kuwait.population * kuwait.gdpPerCapita
plt.plot(kuwait.year, kuwait_gdp / kuwait_gdp.iloc[0] * 100)
plt.plot(kuwait.year, kuwait.gdpPerCapita / kuwait.gdpPerCapita.iloc[0] * 100)

plt.title('GDP and Population Growth in Kuwait (first year = 100)')
plt.legend(['Population', 'GDP', 'GDP per Capita'])
plt.show()

#### Intperpreting the graph:
####  Up until the early 1970s, the population and the GDP grew at roughly 
####   the same rate, but after that, the population kept growing but the 
####   GDP never caught up with it.

print(top_5)

### Comparing how the GDP of the US compared over time with the GDP of 
###  Kuwait.

us = data[data.country == 'United States']

plt.plot(us.year, us.gdpPerCapita)
plt.plot(kuwait.year, kuwait.gdpPerCapita)
plt.legend(['United States', 'Kuwait'])
plt.show()

### Interpreting the graph:
###  The GDP in Kuwait was much higher than the United States until the 
###   1980s, but since then it has been much closer to that of the United 
###   States.

### Therefore, the richest country on a per person basis is not clear a clear
###  cut.
