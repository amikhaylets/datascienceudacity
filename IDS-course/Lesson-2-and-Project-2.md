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
