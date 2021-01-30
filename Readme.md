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