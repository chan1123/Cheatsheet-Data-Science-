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

# Change the style of the figure to the "dark" theme
sns.set_style("dark")

```
#### Summary

- **Trends** - A trend is defined as a pattern of change.
    - `sns.lineplot` - **Line charts** are best to show trends over a period of time, and multiple lines can be used to show trends in more than one group.
- **Relationship** - There are many different chart types that you can use to understand relationships between variables in your data.
    - `sns.barplot` - **Bar charts** are useful for comparing quantities corresponding to different groups.
    - `sns.heatmap` - **Heatmaps** can be used to find color-coded patterns in tables of numbers.
    - `sns.scatterplot` - **Scatter plots** show the relationship between two continuous variables; if color-coded, we can also show the relationship with a third [categorical variable](https://en.wikipedia.org/wiki/Categorical_variable).
    - `sns.regplot` - Including a **regression line** in the scatter plot makes it easier to see any linear relationship between two variables.
    - `sns.lmplot` - This command is useful for drawing multiple regression lines, if the scatter plot contains multiple, color-coded groups.
    - `sns.swarmplot` - **Categorical scatter plots** show the relationship between a continuous variable and a categorical variable.
- **Distribution** - We visualize distributions to show the possible values that we can expect to see in a variable, along with how likely they are.
    - `sns.distplot` - **Histograms** show the distribution of a single numerical variable.
    - `sns.kdeplot` - **KDE plots** (or **2D KDE plots**) show an estimated, smooth distribution of a single numerical variable (or two numerical variables).
    - `sns.jointplot` - This command is useful for simultaneously displaying a 2D KDE plot with the corresponding KDE plots for each individual variable.

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
