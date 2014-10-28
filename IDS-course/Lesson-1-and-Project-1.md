# Lesson 1 and Project 1
***

### Task 1
```python

from pandas import DataFrame, Series

def create_dataframe():
    '''
    Create a pandas dataframe called 'olympic_medal_counts_df' containing
    the data from the  table of 2014 Sochi winter olympics medal counts.  

    The columns for this dataframe should be called 
    'country_name', 'gold', 'silver', and 'bronze'.  

    There is no need to  specify row indexes for this dataframe 
    (in this case, the rows will  automatically be assigned numbered indexes).
    '''

    countries = ['Russian Fed.', 'Norway', 'Canada', 'United States',
                 'Netherlands', 'Germany', 'Switzerland', 'Belarus',
                 'Austria', 'France', 'Poland', 'China', 'Korea', 
                 'Sweden', 'Czech Republic', 'Slovenia', 'Japan',
                 'Finland', 'Great Britain', 'Ukraine', 'Slovakia',
                 'Italy', 'Latvia', 'Australia', 'Croatia', 'Kazakhstan']

    gold = [13, 11, 10, 9, 8, 8, 6, 5, 4, 4, 4, 3, 3, 2, 2, 2, 1, 1, 1, 1, 1, 0, 0, 0, 0, 0]
    silver = [11, 5, 10, 7, 7, 6, 3, 0, 8, 4, 1, 4, 3, 7, 4, 2, 4, 3, 1, 0, 0, 2, 2, 2, 1, 0]
    bronze = [9, 10, 5, 12, 9, 5, 2, 1, 5, 7, 1, 2, 2, 6, 2, 4, 3, 1, 2, 1, 0, 6, 2, 1, 0, 1]

    olympic_medal_counts_df = DataFrame(
	    {'country_name': countries,
	      'gold': gold,
	      'silver': silver,
	      'bronze': bronze}) 

    return olympic_medal_counts_df
```

### Pandas vectorized methods
```python
from pandas import DataFrame, Series

#create data frame
d = {'one': Series([1, 2, 3], index = ['a', 'b', 'c']),
	'two': Series([1, 2, 3, 4], index['a', 'b', 'c', 'd'])}

df = DataFrame(d)

print(df)
```

### Task 2
```python
from pandas import DataFrame, Series
import numpy


def avg_medal_count():
    '''
    Compute the average number of bronze medals earned by countries who 
    earned at least one gold medal.  
    '''

    countries = ['Russian Fed.', 'Norway', 'Canada', 'United States',
                 'Netherlands', 'Germany', 'Switzerland', 'Belarus',
                 'Austria', 'France', 'Poland', 'China', 'Korea', 
                 'Sweden', 'Czech Republic', 'Slovenia', 'Japan',
                 'Finland', 'Great Britain', 'Ukraine', 'Slovakia',
                 'Italy', 'Latvia', 'Australia', 'Croatia', 'Kazakhstan']

    gold = [13, 11, 10, 9, 8, 8, 6, 5, 4, 4, 4, 3, 3, 2, 2, 2, 1, 1, 1, 1, 1, 0, 0, 0, 0, 0]
    silver = [11, 5, 10, 7, 7, 6, 3, 0, 8, 4, 1, 4, 3, 7, 4, 2, 4, 3, 1, 0, 0, 2, 2, 2, 1, 0]
    bronze = [9, 10, 5, 12, 9, 5, 2, 1, 5, 7, 1, 2, 2, 6, 2, 4, 3, 1, 2, 1, 0, 6, 2, 1, 0, 1]
    
	olympic_medal_counts = {'country_name': countries,
	      				'gold': gold,
	      				'silver': silver,
	      				'bronze': bronze} 

	olympic_medal_counts_df = DataFrame(olympic_medal_counts)
	
	bronze_at_least_one_gold = olympic_medal_counts_df['bronze'][olympic_medal_counts_df['gold'] >= 1]

	avg_bronze_at_least_one_gold = numpy.mean(bronze_at_least_one_gold)

    return avg_bronze_at_least_one_gold
```

### Task 3
```python
def avg_medal_count():
    '''
    Using the dataframe's apply method, create a new Series called 
    avg_medal_count that indicates the average number of gold, silver,
    and bronze medals earned amongst countries who earned at 
    least one medal at the 2014 Sochi olympics.
    '''

    countries = ['Russian Fed.', 'Norway', 'Canada', 'United States',
                 'Netherlands', 'Germany', 'Switzerland', 'Belarus',
                 'Austria', 'France', 'Poland', 'China', 'Korea', 
                 'Sweden', 'Czech Republic', 'Slovenia', 'Japan',
                 'Finland', 'Great Britain', 'Ukraine', 'Slovakia',
                 'Italy', 'Latvia', 'Australia', 'Croatia', 'Kazakhstan']

    gold = [13, 11, 10, 9, 8, 8, 6, 5, 4, 4, 4, 3, 3, 2, 2, 2, 1, 1, 1, 1, 1, 0, 0, 0, 0, 0]
    silver = [11, 5, 10, 7, 7, 6, 3, 0, 8, 4, 1, 4, 3, 7, 4, 2, 4, 3, 1, 0, 0, 2, 2, 2, 1, 0]
    bronze = [9, 10, 5, 12, 9, 5, 2, 1, 5, 7, 1, 2, 2, 6, 2, 4, 3, 1, 2, 1, 0, 6, 2, 1, 0, 1]
    
    # your code here
    olympic_medal_counts = {'country_name': countries,
	      					'gold': gold,
	      					'silver': silver,
	      					'bronze': bronze} 

    olympic_medal_counts_df = DataFrame(olympic_medal_counts)

    avg_medal_count = olympic_medal_counts_df[['gold', 'silver', 'bronze']].apply(numpy.mean)
    
    return avg_medal_count
```

### Task 4
```python
import numpy
from pandas import DataFrame, Series


def numpy_dot():
    '''
    Imagine a point system in which each country is awarded 4 points for each
    gold medal,  2 points for each silver medal, and one point for each 
    bronze medal.  

    Using the numpy.dot function, create a new dataframe called 
    'olympic_points_df' that includes:
        a) a column called 'country_name' with the country name
        b) a column called 'points' with the total number of points the country
           earned at the Sochi olympics.
    '''

    countries = ['Russian Fed.', 'Norway', 'Canada', 'United States',
                 'Netherlands', 'Germany', 'Switzerland', 'Belarus',
                 'Austria', 'France', 'Poland', 'China', 'Korea', 
                 'Sweden', 'Czech Republic', 'Slovenia', 'Japan',
                 'Finland', 'Great Britain', 'Ukraine', 'Slovakia',
                 'Italy', 'Latvia', 'Australia', 'Croatia', 'Kazakhstan']

    gold = [13, 11, 10, 9, 8, 8, 6, 5, 4, 4, 4, 3, 3, 2, 2, 2, 1, 1, 1, 1, 1, 0, 0, 0, 0, 0]
    silver = [11, 5, 10, 7, 7, 6, 3, 0, 8, 4, 1, 4, 3, 7, 4, 2, 4, 3, 1, 0, 0, 2, 2, 2, 1, 0]
    bronze = [9, 10, 5, 12, 9, 5, 2, 1, 5, 7, 1, 2, 2, 6, 2, 4, 3, 1, 2, 1, 0, 6, 2, 1, 0, 1]
 
    # YOUR CODE HERE
    olympic_medal_counts = {'country_name': countries,
	      					'gold': gold,
	      					'silver': silver,
	      					'bronze': bronze} 

    olympic_medal_counts_df = DataFrame(olympic_medal_counts)

    medal_counts = olympic_medal_counts_df[['gold', 'silver', 'bronze']]
    points = numpy.dot(medal_counts, [4, 2, 1])

    olympic_points = {'country_name': Series(countries), 'points': Series(points)}
    olympic_points_df = DataFrame(olympic_points)
    
    return olympic_points_df
```
