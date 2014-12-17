# Lesson 3 and Project 3
***

### Welch's t-test
```python
import numpy
import scipy.stats
import pandas

def compare_averages():
	"""
	Performs a t-test on two sets of baseball data (left-handed and right-handed hitters)
	"""
	# read data into pandas dataframe
	baseball_data = pandas.read_csv('../data/baseball_data.csv')

	# split the data set into two data frames - left-handed and right-handed
	baseball_data_left = baseball_data[baseball_data['handedness'] == 'L']
	baseball_data_right = baseball_data[baseball_data['handedness'] == 'R']

	# perfrom wlch's t-test
	result = scipy.stats.ttest_ind(baseball_data_left['avg'], baseball_data_right['avg'], equal_var=False)

	# produce desired result
	if result[1] <= 0.05:
		return (False, result)
	else:
		return (True, result)
```
