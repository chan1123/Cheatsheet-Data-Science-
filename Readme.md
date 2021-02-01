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
