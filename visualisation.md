# Data Visualisation

#### Required Libraries
```python
import pandas as pd
pd.plotting.register_matplotlib_converters()
import matplotlib.pyplot as plt
%matplotlib inline
import seaborn as sns
```
#### Overheads

```python
# Set the width and height of the figure
plt.figure(figsize=(14,6))

# Add title
plt.title("Desired Title")

# Add label for horizontal axis
plt.xlabel("x-axis")

# Add label for vertical axis
plt.ylabel("y-axis")
```


#### Line
```python
# Set the width and height of the figure
plt.figure(figsize=(16,6))

# Line chart showing df_data
sns.lineplot(data=df_data)

# Line chart showing first line
sns.lineplot(data=df['first'], label="first")

# Line chart showing second line
sns.lineplot(data=df['second'], label='second')
```
