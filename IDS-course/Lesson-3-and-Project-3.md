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

### Gradient Descent in Python
```python
import numpy
import pandas

def compute_cost(features, values, theta):
    """
    Compute the cost of a list of parameters, theta, given a list of features (input
    data points) and values (output data points).
    """
    m = len(values)
    sum_of_square_errors = numpy.square(numpy.dot(features, theta) - values).sum()
    cost = sum_of_square_errors / (2*m)

    return cost

def gradient_descent(features, values, theta, alpha, num_iterations):
    """
    Perform gradient descent given a data set with an arbitrary number of features.
    """

    cost_history = []

    m = len(values) * 1.0
    
    for i in range(0,num_iterations):
    	# Calculate cost
	cost = compute_cost(features, values, theta)
	
	# Append cost to history
	cost_history.append(cost)
	
	# Calculate new theta
	theta = theta + alpha * (1/m) * numpy.dot((values - numpy.dot(features,theta)),features)

    return theta, pandas.Series(cost_history) 
```

### Calculate R^2
```python
import numpy as np

def compute_r_squared(data, predictions):
    # Function that, given two input numpy arrays, 'data', and 'predictions,'
    # returns the coefficient of determination, R^2, for the model that produced 

    mean = np.mean(data)
    a = np.sum(np.square(data - predictions))
    b = np.sum(np.square(data - mean))
    r_squared = 1.0 - (a/b)

    return r_squared
```

### Project 3.1 - Exploratory Data Analysis
```python
import numpy as np
import pandas
import matplotlib.pyplot as plt

def entries_histogram(turnstile_weather):
    '''
    Plot two histograms on the same axes to show hourly
    entries when raining vs. when not raining. Here's an example on how
    to plot histograms with pandas and matplotlib
    
    You can see the information contained within the turnstile weather data here:
    https://www.dropbox.com/s/meyki2wl9xfa7yk/turnstile_data_master_with_weather.csv
    '''
    
    plt.figure()
    rain = turnstile_weather["ENTRIESn_hourly"][turnstile_weather["rain"]==1].hist()
    no_rain = turnstile_weather["ENTRIESn_hourly"][turnstile_weather["rain"]==0].hist()
    
    return plt
```
