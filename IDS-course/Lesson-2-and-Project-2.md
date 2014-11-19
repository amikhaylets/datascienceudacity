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
