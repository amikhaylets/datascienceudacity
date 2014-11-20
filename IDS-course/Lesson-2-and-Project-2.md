# Lesson 2 and Project 2
***

### Task 1

```python
import pandas

def add_full_name(path_to_csv, path_to_new_csv):
    '''reads a csv located at "path_to_csv" and write it into a pandas dataframe and adds a new column
    called 'nameFull' with a player's full name.
    For example: for Hank Aaron, nameFull would be 'Hank Aaron'
    '''
    
    df = pandas.read_csv(path_to_csv)
    df['nameFull'] = df['nameFirst'] + " " + df['nameLast']
    df.to_csv(path_to_new_csv)
    
```

### Task 2

```python
import pandas
import pandasql

def select_first_50(filename):
    # Read in our aadhaar_data csv to a pandas dataframe.  Afterwards, we rename the columns
    # by replacing spaces with underscores and setting all characters to lowercase, so the
    # column names more closely resemble columns names one might find in a table.
    aadhaar_data = pandas.read_csv(filename)
    aadhaar_data.rename(columns = lambda x: x.replace(' ', '_').lower(), inplace=True)

    # Select out the first 50 values for "registrar" and "enrolment_agency"
    # in the aadhaar_data table using SQL syntax. 

    q = """
    SELECT 
    registrar, enrolment_agency 
    FROM 
    aadhaar_data 
    LIMIT 50;
    """

    #Execute SQL command against the pandas frame
    aadhaar_solution = pandasql.sqldf(q.lower(), locals())
    return aadhaar_solution  
```

### Task 3

```python
import pandas
import pandasql

def aggregate_query(filename):
    # Read aadhaar_data csv to a pandas dataframe.  Afterwards, rename the columns
    # by replacing spaces with underscores and setting all characters to lowercase, so the
    # column names more closely resemble columns names one might find in a table.
    
    aadhaar_data = pandas.read_csv(filename)
    aadhaar_data.rename(columns = lambda x: x.replace(' ', '_').lower(), inplace=True)

    # a query selects from the aadhaar_data table how many men and how 
    # many women over the age of 50 have had aadhaar generated for them in each district

    # The possible columns to select from aadhaar data are:
    #     1) registrar
    #     2) enrolment_agency
    #     3) state
    #     4) district
    #     5) sub_district
    #     6) pin_code
    #     7) gender
    #     8) age
    #     9) aadhaar_generated
    #     10) enrolment_rejected
    #     11) residents_providing_email,
    #     12) residents_providing_mobile_number
    #
    # A copy of the aadhaar data passing into this exercise below:
    # https://www.dropbox.com/s/vn8t4uulbsfmalo/aadhaar_data.csv
        
    q = """
    SELECT 
    gender, district, sum(aadhaar_generated) 
    FROM 
    aadhaar_data 
    WHERE 
    age>50 
    GROUP BY 
    gender, district;
    """

    # Execute SQL command against the pandas frame
    aadhaar_solution = pandasql.sqldf(q.lower(), locals())
    return aadhaar_solution    
```

### Project 2 Task 1 (2.1)

```python
import pandas
import pandasql


def num_rainy_days(filename):
    '''
    Function run a SQL query on a dataframe of
    weather data.  The SQL query return one column and
    one row - a count of the number of days in the dataframe where
    the rain column is equal to 1 (i.e., the number of days it
    rained).
    
    SQL count fuction:
    https://dev.mysql.com/doc/refman/5.1/en/counting-rows.html
    
    The weather data that we are passing in below:
    https://www.dropbox.com/s/7sf0yqc9ykpq3w8/weather_underground.csv
    '''
    weather_data = pandas.read_csv(filename)

    q = """
    SELECT
    COUNT(*)
    FROM
    weather_data
    WHERE
    rain > 0;
    """
    
    #Execute SQL command against the pandas frame
    rainy_days = pandasql.sqldf(q.lower(), locals())
    return rainy_days

```
