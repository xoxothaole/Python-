# COVID-19 Analysis 
### Objective 
With a background in microbiology and experience in the pharmacy setting, I've witnessed how public health data represents far more than numbers — it reflects real people, real outcomes. This project was inspired by the stark disparities I observed in healthcare infrastructure and pandemic response between countries during COVID-19. Using Python for comprehensive data analysis and visualization, this project examines global COVID-19 patterns to reveal inequities in testing access, healthcare outcomes, and pandemic preparedness across different regions. The analysis goes beyond surface-level statistics to uncover meaningful insights about global health disparities, with the goal of presenting data-driven evidence that can inform public health policy and highlight areas where targeted interventions are most needed.

### Context 
This project leverages multiple datasets to provide data-driven insights into the pandemic's progression, helping visualize how different regions were affected and how key metrics evolved over time. This analysis is particulary valuable for understanding testing effciency, mortality patterns, and recovery trends acorss different geographical regions and WHO-defined areas. 

### Tools and Methodologies 
- Python- Primary programming language for data analysis
- Pandas- Data manipulation, cleaning, and analysis
- NumPy- Numerical computing and array operations
- Plotly- Interactive data visualizations and dashboard creation
- Matplotlib- Additional plotting and visualization support
- WordCloud- Text visualization for COVID-related conditions

### Data Sources
- covid- This dataset contains Country/Region, Continent,  Population, TotalCases, NewCases, TotalDeaths, NewDeaths,  TotalRecovered, NewRecovered, ActiveCases, Serious, Critical, Tot Cases/1M pop, Deaths/1M pop, TotalTests, Tests/1M pop, WHO Region, iso_alpha
- covid_grouped- This dataset contains Date(from 20-01-22 to 20-07-27), Country/Region, Confirmed, Deaths, Recovered, Active, New cases, New deaths, New recovered, WHO Region, iso_alpha.
- coviddeath- This dataset contains real-world examples of a number of Covid-19 deaths and the reasons behind the deaths

### Data Analysis and Process
```python
# Data analysis and manipulation
import plotly.graph_objs as go
import plotly.io as pio
import plotly.express as px
import pandas as pd
import numpy

# Data Visualization 
import matplotlib.pyplot as plt

# Importing Plotly
import plotly.offline as py 
py.init_notebook_mode(connected=True)

#Initializing Plotly
pio.renderers.default = 'colab'

# Importing Dataset1
import os
print("Current working directory:", os.getcwd())
os.chdir('/Users/thaole/Downloads')
print("New working directory:", os.getcwd())

dataset1 = pd.read_csv("covid.csv")
dataset1.head() # returns first 5 rows
```
> <small>* The initial data check reveals the comprehensive global coverage with key metrics needed to examine healthcare infrastructure differences. However, the data exploration also pointed out that there are significant mising values in testing data across several countries. It is likley that countries with incomplete testing data likley reflect limited healthcare infrastructure, which means focusing on countries with complete datasets will give me the most meaningful comparisons for understanding pandemic response effectiveness.* <small>

```python
# Returns tuple of shape (Rows, columns)
print(dataset1.shape)

# Returns size of dataframe (total numbers of cells)
print(dataset1.size)

# Information about Dataset1
# return concise summary of dataframe 
dataset1.info()

# Importing Dataset2
dataset2 = pd.read_csv("covid_grouped.csv")
dataset2.head # return first 5 rows of dataset2

# Return tuple of shape (Rows, columns)
print(dataset2.shape)
print(dataset2.size)

# Information about Dataset2
dataset2.info() # return concise summary of dataframe

# Columns labels of a Dataset1
dataset1.columns

# Data cleaning - drop NewCases, NewDeaths, NewRecovered rows from dataset1
dataset1.drop(['NewCases', 'NewDeaths', 'NewRecovered'], axis = 1, inplace = True)

# Select random set of values from dataset1
dataset1.sample(5)

# Import create_table Figure Factory

from plotly.figure_factory import create_table

colorscale = [[0, '#4d004c'], [.5, '#f2e5ff'], [1, '#ffffff']]
table = create_table(dataset1.head(15), colorscale=colorscale)
table.show()
```
> <small> Performed data structure validation using shape, size, and info() methods across both datasets. Removing daily change columns eliminates noise and focuses analysis on aggregate outcomes. <small>

```python
px.bar(dataset1.head(15), x = 'Country/Region',
       y = 'TotalCases', color = 'TotalCases',
       height = 500, hover_data = ['Country/Region', 'Continent'])

px.bar(dataset1.head(15), x = 'Country/Region', y = 'TotalCases', 
       color = 'TotalDeaths', height = 500,
       hover_data = ['Country/Region', 'Continent'])

px.bar(dataset1.head(15), x = 'Country/Region', y = 'TotalCases', 
       color = 'TotalRecovered', height = 500,
       hover_data = ['Country/Region', 'Continent'])

px.bar(dataset1.head(15), x = 'Country/Region', y = 'TotalCases',
       color = 'TotalTests', height = 500, hover_data = ['Country/Region', 'Continent'])

px.bar(dataset1.head(15),  x = 'TotalTests', y = 'Country/Region',
       color = 'TotalTests', orientation = 'h', height = 500, 
       hover_data = ['Country/Region', 'Continent'])

px.bar(dataset1.head(15), x = 'TotalTests', y = 'Continent',
       color = 'TotalTests', orientation = 'h', height = 500,
       hover_data = ['Country/Region', 'Continent'])

px.scatter(dataset1, x = 'Continent', y = 'TotalCases',
           hover_data = ['Country/Region', 'Continent'],
           color = 'TotalCases', size = 'TotalCases', size_max = 80)

px.scatter(dataset1.head(57), x = 'Continent', y = 'TotalCases', 
           hover_data = ['Country/Region', 'Continent'],
           color = 'TotalCases', size = 'TotalCases', size_max = 80, log_y = True)

px.scatter(dataset1.head(54), x = 'Continent', y = 'TotalTests', 
           hover_data = ['Country/Region', 'Continent'], color = 'TotalTests',
           size = 'TotalTests', size_max = 80)

px.scatter(dataset1.head(50), x = 'Continent', y = 'TotalTests', 
           hover_data = ['Country/Region', 'Continent'], 
           color = 'TotalTests', size = 'TotalTests', size_max = 80, log_y = True)

px.scatter(dataset1.head(100), x = 'Country/Region', y = 'TotalCases', 
           hover_data = ['Country/Region', 'Continent'],
           color = 'TotalCases', size = 'TotalCases', size_max = 80)

px.scatter(dataset1.head(30), x = 'Country/Region', y = 'TotalCases',
           hover_data = ['Country/Region', 'Continent'],
           color = 'Country/Region', size = 'TotalCases', size_max = 80, log_y = True)

px.scatter(dataset1.head(10), x = 'Country/Region', y = 'TotalDeaths', 
           hover_data = ['Country/Region', 'Continent'],
           color = 'Country/Region', size = 'TotalDeaths', size_max = 80)

px.scatter(dataset1.head(10), x = 'Country/Region', y = 'TotalDeaths',
           hover_data = ['Country/Region', 'Continent'],
           color = 'Country/Region', size = 'TotalDeaths', size_max = 80)

px.scatter(dataset1.head(30), x = 'Country/Region', y = 'Tests/1M pop', 
           hover_data = ['Country/Region', 'Continent'],
           color = 'Country/Region', size = 'Tests/1M pop', size_max = 80)

px.scatter(dataset1.head(30), x = 'TotalCases', y = 'TotalDeaths',
           hover_data = ['Country/Region', 'Continent'],
           color = 'TotalDeaths', size = 'TotalDeaths', size_max = 80)

px.scatter(dataset1.head(30), x = 'TotalCases', y = 'TotalDeaths', 
           hover_data = ['Country/Region', 'Continent'],
           color = 'TotalDeaths', size = 'TotalDeaths', size_max = 80,
           log_x = True, log_y = True)

px.scatter(dataset1.head(30), x = 'TotalTests', y = 'TotalCases', 
           hover_data = ['Country/Region', 'Continent'],
           color = 'TotalTests', size = 'TotalTests', size_max = 80,
           log_x = True, log_y = True)

px.bar(dataset2, x = "Date", y = "Confirmed", color = "Confirmed", 
       hover_data = ["Confirmed", "Date", "Country/Region"], height = 400)

px.bar(dataset2, x = "Date", y = "Confirmed", color = "Confirmed", 
       hover_data = ["Confirmed", "Date", "Country/Region"], log_y = True, height = 400)

px.bar(dataset2, x = "Date", y = "Deaths", color = "Deaths",
       hover_data = ["Confirmed", "Date", "Country/Region"],
       log_y = False, height = 400)

# United States Specific Analysis 

df_US = dataset2.loc[dataset2["Country/Region"] == "US"]

px.bar(df_US, x = "Date", y = "Confirmed", color = "Confirmed", height = 400)
