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
    
    The weather data below:
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

### Project 2 Task 2 (2.2)

```python
import pandas
import pandasql


def max_temp_aggregate_by_fog(filename):
    '''
    Function run a SQL query on a dataframe of
    weather data.  The SQL query return two columns and
    two rows - whether it was foggy or not (0 or 1) and the max
    maxtempi for that fog value (i.e., the maximum max temperature
    for both foggy and non-foggy days).
    
    Weather data below:
    https://www.dropbox.com/s/7sf0yqc9ykpq3w8/weather_underground.csv
    '''
    weather_data = pandas.read_csv(filename)

    q = """
    SELECT
    fog,
    max(cast (maxtempi as integer))
    FROM
    weather_data
    GROUP BY
    fog;
    """
    
    #Execute SQL command against the pandas frame
    rainy_days = pandasql.sqldf(q.lower(), locals())
    return rainy_days

```

### Project 2 Task 3 (2.3)

```python
import pandas
import pandasql

def avg_min_temperature(filename):
    '''
    Function run a SQL query on a dataframe of
    weather data.  The SQL query return one column and
    one row - the average meantempi on days that are a Saturday
    or Sunday (i.e., the the average mean temperature on weekends).
    The dataframe will be titled 'weather_data' and you can access
    the date in the dataframe via the 'date' column.
    
    Weather data below:
    https://www.dropbox.com/s/7sf0yqc9ykpq3w8/weather_underground.csv
    '''
    weather_data = pandas.read_csv(filename)

    q = """
    SELECT
    avg(cast (meantempi as integer))
    FROM
    weather_data
    WHERE
    cast (strftime('%w', date) as integer) = 0
    OR
    cast (strftime('%w', date) as integer) = 6;
    """
    
    #Execute SQL command against the pandas frame
    mean_temp_weekends = pandasql.sqldf(q.lower(), locals())
    return mean_temp_weekends
```

### Project 2 Task 4 (2.4) Mean Temp on Rainy Days

```python
import pandas
import pandasql

def avg_min_temperature(filename):
    '''
    Function run a SQL query on a dataframe of
    weather data. Finds the average minimum temperature on rainy days where the 
    minimum temperature is greater than 55 degrees.

    Weather data below:
    https://www.dropbox.com/s/7sf0yqc9ykpq3w8/weather_underground.csv
    '''
    weather_data = pandas.read_csv(filename)

    q = """
    SELECT
    avg(cast (mintempi as integer))
    FROM
    weather_data
    WHERE
    mintempi > 55
    AND
    rain > 0;
    """
    
    #Execute SQL command against the pandas frame
    mean_temp_weekends = pandasql.sqldf(q.lower(), locals())
    return mean_temp_weekends
```

### Project 2 Task 5 (2.5) Fixing Turnstile Data

```python
import csv

def fix_turnstile_data(filenames):
    '''
    Filenames is a list of MTA Subway turnstile text files. A link to an example
    MTA Subway turnstile text file can be seen at the URL below:
    http://web.mta.info/developers/data/nyct/turnstile/turnstile_110507.txt
    
    Function will update each row in the text file so there is only one entry per row. 
    A few examples below:
    A002,R051,02-00-00,05-28-11,00:00:00,REGULAR,003178521,001100739
    A002,R051,02-00-00,05-28-11,04:00:00,REGULAR,003178541,001100746
    A002,R051,02-00-00,05-28-11,08:00:00,REGULAR,003178559,001100775
    
    Write the updates to a different text file in the format of "updated_" + filename.
    For example:
        1) if you read in a text file called "turnstile_110521.txt"
        2) you should write the updated data to "updated_turnstile_110521.txt"

    The order of the fields should be preserved. 
    
    You can see a sample of the turnstile text file that's passed into this function
    and the the corresponding updated file in the links below:
    
    Sample input file:
    https://www.dropbox.com/s/mpin5zv4hgrx244/turnstile_110528.txt
    Sample updated file:
    https://www.dropbox.com/s/074xbgio4c39b7h/solution_turnstile_110528.txt
    '''
    for name in filenames:
        with open(name, 'rb') as fin:
            reader = csv.reader(fin)
            with open('updated_' + name, 'ab') as fout:
                writer = csv.writer(fout)
                for row in reader:
                    header = row[0:3]
                    row = row[3:len(row)]
                    while len(row) > 0:
                        writer.writerow(header + row[0:5])
                        row = row[5:len(row)]

```
