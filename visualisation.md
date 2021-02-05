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
# Line chart showing df_data
sns.lineplot(data=df_data)

# Line chart showing first line
sns.lineplot(data=df['first'], label="first")

# Line chart showing second line
sns.lineplot(data=df['second'], label='second')
```

#### Bar
Note: Horizontal bar chart if the label involves words ** Switch x and y axis
```python
sns.barplot(x=df.index, y=df['column'])
```
#### Heatmap

```python
sns.heatmap(data=df, annot=True)
# annot=True ensures that the values for each cell appear on the chart
```

#### Scatter plots (Regression plot)

##### 2 variables
```python
sns.scatterplot(x=x_axis, y=y_axis)

# check the strength of the relationship
# add a regression line

sns.regplot(x=x_axis, y=y_axis)
```
##### 3 variables
Display relationships between 3 variables by color coding the points

```python
sns.scatterplot(x=x_axis, y=y_axis, hue=third_variable)

# add 2 regression lines
# Note: lmplot is read as lineplot
sns.lmplot(x=x_name, y=y_name, hue=third_variable, data=insurance_data)
```
##### Categorical scatter plot

```python
# usually x-axis is a boolean/categorical
sns.swarmplot(x=x_axis,
              y=y_axis)
```

#### Histogram

```python
sns.distplot(a=df[column], kde=False)
```

#### Density plots (1D)
kernel density estimate (KDE) plot

```python
sns.kdeplot(data=df[column], shade=True)
```

#### 2D KDE plots

```python
sns.jointplot(x=df[column], y=df[column], kind="kde")
```

#### 2 or more histograms (or kde)

```python
sns.distplot(a=df[a]], label="a", kde=False)
sns.distplot(a=df[b]], label="b", kde=False)
sns.distplot(a=df[c]], label="c", kde=False)

# Add title
plt.title("Desired Title")

# Force legend to appear
plt.legend()
```
