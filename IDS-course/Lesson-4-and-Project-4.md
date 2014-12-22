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
