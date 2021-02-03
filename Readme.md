# Data Cleaning Cheatsheet

### Check documentation of the dataset
### Check missing values


```python
# First 5 rows
df.head()

# Get number of missing data points per column
df.isnull().sum()

# Find percentage of missing values
total_cells = np.product(df.shape)
total_missing = df.isnull().sum()
percentage = total_missing/total_cells
```
#### Is this value missing because it wasn't recorded or because it doesn't exist?

#### Drop missing values (not recommended)
```python
# remove all the rows that contain a missing value
print('Original rows:', df.shape[0])
dropped = df.dropna()
print('Remaining rows:', dropped.shape[0])

# remove all columns with at least one missing value
print('Original columns:', df.shape[1])
dropped = df.dropna(axis=1)
print('Remaining columns', dropped.shape[1])
```

#### Fill missing values

```python
# replace all NA's with 0
df.fillna(0)

# replace all NA's the value that comes directly after it in the same column, 
# then replace all the remaining na's with 0
df.fillna(method='bfill', axis=0).fillna(0)
```

### Scaling and normalization
#### Definition: 
- Scaling: Changing range of data
- Normalisation: Changing shape of the distribution of data

#### Required libraries

```python
import pandas as pd
import numpy as np

# for Box-Cox Transformation
from scipy import stats

# for min_max scaling
from mlxtend.preprocessing import minmax_scaling

# plotting modules
import seaborn as sns
import matplotlib.pyplot as plt
```

#### Scaling
Commonly used before applying support vector machine or k-nearest neighbours. 

```python
# generate 1000 data points randomly drawn from an exponential distribution
original_data = np.random.exponential(size=1000)

# mix-max scale the data between 0 and 1
scaled_data = minmax_scaling(original_data, columns=[0])
```

#### Normalisation
Assumes the data is normally distributed

```python
# normalize the exponential data with boxcox
normalized_data = stats.boxcox(original_data)
```

### Parsing Dates
Tell pandas to use ```datetime64``` dtypes
#### Required libraries

```python
import pandas as pd
import numpy as np
import seaborn as sns
import datetime
```

#### Check date lengths
```python
# Check the length of the entries
date_lengths = df['Dates'].str.len()
date_lengths.value_counts()

# Check where is the issue
indices = np.where([date_lengths == **])[1]
print('Indices with corrupted data:', indices)
earthquakes.loc[indices]
```

#### Convert date columns to datetime

```python
# create a new column, date_parsed, with the parsed dates
df['date_parsed'] = pd.to_datetime(df['date'], format="%m/%d/%y")
```
#### Ask pandas to be smart to infer
Why not advisable?
- Slow
- Someone is creative with the date entry and result is not always correct
```python
df['date_parsed'] = pd.to_datetime(df['Date'], infer_datetime_format=True)
```

#### Select the day
Note: ```.dt``` is attribute of the datetime object
```python
day_of_month = df['date_parsed'].dt.day
day_of_month.head()
```

#### Check for errors to see if we mix up months and date
Check if dates are distributed from 1 to 31
```python
# remove na's
day_of_month = day_of_month.dropna()

# plot the day of the month
sns.distplot(day_of_month, kde=False, bins=31)
```

### Dealing with Character Encodings issue

#### Required Libraries

```python
import pandas as pd
import numpy as np

# helpful character encoding module
import chardet
```

#### Check what is the encoding
Warning: chardet is just guessing. 
```python
# look at the first ten thousand bytes to guess the character encoding
with open(filename, 'rb') as rawdata:
    result = chardet.detect(rawdata.read(1_000_000))

# check what the character encoding might be
print(result)

pd.read_csv(filename, encoding=**)
```

#### Save the file (automatically UTF-8)

```python
df.to_csv(filename)
```

### Cleaning inconsistent data entry

#### Required Libraries
```python
import pandas as pd
import numpy as np

# helpful modules
import fuzzywuzzy
from fuzzywuzzy import process
import chardet
```

#### Inconsistent Text
Check for inconsistent data entry
1. make everything lower case
2. remove white spaces at the beginning and end
```python
# get all the unique values
unique_text = df['column of interest'].unique()
unique_text.sort()

df['column of interest'] = df['column of interest'].str.lower()

df['column of interest'] = df['column of interest'].str.strip()

```

#### Use fuzzy matching to correct inconsistent data entry
Fuzzy matching: The process of automatically finding text strings that are very similar to the target string

```python
# get the top 10 closest matches to target_string
matches = fuzzywuzzy.process.extract(target_string, list_of_strings, limit=10, scorer=fuzzywuzzy.fuzz.token_sort_ratio)

# take a look at them
matches
```
```python
# convert to lower case
professors['Country'] = professors['Country'].str.lower()
# remove trailing white spaces
professors['Country'] = professors['Country'].str.strip()

# get the top 10 closest matches to "south korea"
countries = professors['Country'].unique()
matches = fuzzywuzzy.process.extract("south korea", countries, limit=10, scorer=fuzzywuzzy.fuzz.token_sort_ratio)

def replace_matches_in_column(df, column, string_to_match, min_ratio = 47):
    # get a list of unique strings
    strings = df[column].unique()
    
    # get the top 10 closest matches to our input string
    matches = fuzzywuzzy.process.extract(string_to_match, strings, 
                                         limit=10, scorer=fuzzywuzzy.fuzz.token_sort_ratio)

    # only get matches with a ratio > 90
    close_matches = [matches[0] for matches in matches if matches[1] >= min_ratio]

    # get the rows of all the close matches in our dataframe
    rows_with_matches = df[column].isin(close_matches)

    # replace all rows with close matches with the input matches 
    df.loc[rows_with_matches, column] = string_to_match
    
    # let us know the function's done
    print("All done!")
    
replace_matches_in_column(df=professors, column='Country', string_to_match="south korea")
countries = professors['Country'].unique()
```

