# Data Cleaning Cheatsheet

#### Check documentation
#### Check missing values


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
df.dropna()
print('Remaining rows:', df.shape[0])

# remove all columns with at least one missing value
df.dropna(axis=1)
```