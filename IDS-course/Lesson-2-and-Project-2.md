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

### Project 2 Task 6 (2.6)

```python
def create_master_turnstile_file(filenames, output_file):
    '''
    Function that takes the files in the list filenames, which all have the 
    columns 'C/A, UNIT, SCP, DATEn, TIMEn, DESCn, ENTRIESn, EXITSn', and consolidates
    them into one file located at output_file.  There should be ONE row with the column
    headers, located at the top of the file.
    '''
    
    with open(output_file, 'w') as master_file:
       master_file.write('C/A,UNIT,SCP,DATEn,TIMEn,DESCn,ENTRIESn,EXITSn\n')
       for filename in filenames:
                with open(filename, 'rb') as f:
                    for line in f:
                        master_file.write(line)

```

### Project 2 Task 7 (2.7)

```python
import pandas

def filter_by_regular(filename):
    '''
    Function read the csv file located at filename into a pandas dataframe,
    and filter the dataframe to only rows where the 'DESCn' column has the value 'REGULAR'.
    '''
    
    turnstile_data = pandas.read_csv(filename)
    turnstile_data = turnstile_data[turnstile_data['DESCn'] == 'REGULAR']
    return turnstile_data
```

### Project 2 Task 8 (2.8)

```python
import pandas

def get_hourly_entries(df):
    '''
    The data in the MTA Subway Turnstile data reports on the cumulative
    number of entries and exits per row.  Dataframe called df that contains only 
    the rows for a particular turnstile machine (i.e., unique SCP, C/A, and UNIT).  
    Function change cumulative entry numbers to a count of entries since the last reading
    (i.e., entries since the last row in the dataframe).
    
    More specifically, it does two things:
       1) Create a new column called ENTRIESn_hourly
       2) Assign to the column the difference between ENTRIESn of the current row 
          and the previous row. If there is any NaN, fill/replace it with 1.
    '''
    df['ENTRIESn_hourly'] = df['ENTRIESn'] - df['ENTRIESn'].shift(1)
    df = df.fillna(1)
    return df
```

### Project 2 Task 9 (2.9)

```python
import pandas

def get_hourly_exits(df):
    '''
    The data in the MTA Subway Turnstile data reports on the cumulative
    number of entries and exits per row.  Dataframe called df that contains only 
    the rows for a particular turnstile machine (i.e., unique SCP, C/A, and UNIT).  
    Function change cumulative exit numbers to a count of exits since the last reading
    (i.e., exits since the last row in the dataframe).
    
    More specifically, it does two things:
       1) Create a new column called EXITSn_hourly
       2) Assign to the column the difference between EXITSn of the current row 
          and the previous row. If there is any NaN, fill/replace it with 0.
    '''
    
    df['EXITSn_hourly'] = df['EXITSn'] - df['EXITSn'].shift(periods = 1)
    df = df.fillna(0)
    return df

```

### Project 2 Task 10 (2.10)

```python
import pandas

def time_to_hour(time):
    '''
    Given an input variable time that represents time in the format of:
    00:00:00 (hour:minutes:seconds)
    
    Function to extract the hour part from the input variable time
    and return it as an integer. For example:
        1) if hour is 00, your code should return 0
        2) if hour is 01, your code should return 1
        3) if hour is 21, your code should return 21
    '''
    
    hour = int(time[0:2])
    return hour
```

### Project 2 Task 11 (2.11)

```python
import datetime

def reformat_subway_dates(date):
    '''
    The dates in subway data are formatted in the format month-day-year.
    The dates in weather underground data are formatted year-month-day.
    
    In order to join these two data sets together, we'll want the dates formatted
    the same way.  Function takes as its input a date in the MTA Subway
    data format, and returns a date in the weather underground format.
    '''

    date_formatted = datetime.datetime.strptime(date, '%m-%d-%y').strftime('%Y-%m-%d')
    return date_formatted

```
