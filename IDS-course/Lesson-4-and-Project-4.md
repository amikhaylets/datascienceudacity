# Lesson 4 and Project 4
***

### Plotting in Python
```python
from pandas import *
from ggplot import *

import pandas

def lineplot(hr_year_csv):
    # A csv file will be passed in as an argument which
    # contains two columns - 'HR' (the number of homerun hits)
    # and 'yearID' (the year in which the homeruns were hit).
    #
    # Lineplot, uses the passed-in csv file, hr_year.csv, and create a
    # chart with points connected by lines, both colored 'red',
    # showing the number of HR by year.

    gg = ggplot(aes(x='yearID', y='HR'), data=pandas.read_csv(hr_year_csv)) + \
                geom_point(color='red') + \
                geom_line(color='red')
    return gg
```

### Plotting Line Charts
```python
from pandas import *
from ggplot import *

import pandas

def lineplot_compare(hr_by_team_year_sf_la_csv):
    # Function, lineplot_compare, will read a csv file
    # called hr_by_team_year_sf_la.csv and plot it using pandas and ggplot2.
    
    gg = ggplot(pandas.read_csv(hr_by_team_year_sf_la_csv), aes('yearID', 'HR', color='teamID')) + \
                geom_line()
    return gg
```

### Project 4 Exercise - Visualization 1
```python
from pandas import *
from ggplot import *

def plot_weather_data(turnstile_weather):
    '''
    Here are some suggestions for things to investigate and illustrate:
     * Ridership by time of day or day of week
     * How ridership varies based on Subway station
     * Which stations have more exits or entries at different times of day

    If you'd like to learn more about ggplot and its capabilities, take
    a look at the documentation at:
    https://pypi.python.org/pypi/ggplot/
     
    You can check out:
    https://www.dropbox.com/s/meyki2wl9xfa7yk/turnstile_data_master_with_weather.csv
     
    To see all the columns and data points included in the turnstile_weather 
    dataframe. 
     
    However, due to the limitation of our Amazon EC2 server, we are giving you a random
    subset, about 1/3 of the actual data in the turnstile_weather dataframe.
    '''

    data = turnstile_weather
    entries = data[['DATEn', 'ENTRIESn_hourly']].groupby('DATEn', as_index=False).mean()
    entries['Day'] = [datetime.strptime(x, '%Y-%m-%d').strftime('%w%A')
                      for x in entries['DATEn']]
    entries_Day=entries.groupby('Day', as_index=False)['ENTRIESn_hourly'].mean()
    plot = ggplot(entries_Day, aes(x = 'Day', y='ENTRIESn_hourly')) +\
                geom_bar(weight='ENTRIESn_hourly', fill='blue', stat = "bar") +\
                ggtitle('NYC subway ridership by day') + xlab('Day') + ylab('Entries')

    return plot
```
