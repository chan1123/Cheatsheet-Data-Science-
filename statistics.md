## Data Types

#### Numeric
Data that are expressed on a numeric scale

- Continuous: 
> Data that can take on any value in an interval. (Synonyms: interval, float, numeric)
- Discrete: 
> Data that can take on only integer values, such as counts. (Synonyms: integer, count)

#### Categorical
Data that can take on only a specific set of values representing a set of possible categories. (Synonyms: enums, enumerated, factors, nominal)

- Binary: 
> A special case of categorical data with just two categories of values, e.g., 0/1, true/false. (Synonyms: dichotomous, logical, indicator, boolean)
- Ordinal: 
> Categorical data that has an explicit ordering. (Synonym: ordered factor)

#### Benefit of explicitly identifying the data as categorical
1. Tell software how to produce a chart/fit a model (e.g. In Python, scikit-learn supports ordinal data with the sklearn.preprocessing.OrdinalEncoder)
2. Optimize storage and indexing
3. Enforce the values that can be stored for a given categorical variable. 

## Rectangular data

Definition
> a two-dimensional matrix with rows indicat‐ ing records (cases) and columns indicating features (variables)

#### Dataframe
Rectangular data (like a spreadsheet) is the basic data structure for statistical and machine learning models.

#### Feature
A column within a table is commonly referred to as a feature.

> Synonyms: attribute, input, predictor, variable

#### Outcome
Many data science projects involve predicting an outcome—often a yes/no outcome. The features are sometimes used to predict the outcome in an experiment or a study.

> Synonyms: dependent variable, response, target, output

#### Records
A row within a table is commonly referred to as a record.

> Synonyms: case, example, instance, observation, pattern, sample

## Estimates of Location

#### Mean(average)
The sum of all values divided by the number of values.
```python
df[column].mean()
```

#### Weighted mean (weighted average)
The sum of all values times a weight divided by the sum of the weights.
```python
import numpy as np
np.average(df[column], weights=df[weight])
```
#### Median (50th percentile)
The value such that one-half of the data lies above and below.
```python
df[column].median()
```
#### Percentile (quantile)
The value such that P percent of the data lies below.

#### Weighted median
The value such that one-half of the sum of the weights lies above and below the sorted data.
```python
import wquantiles
wquantiles.median(df[column], weights=df[weight])
```
#### Trimmed mean (truncated mean)
The average of all values after dropping a fixed number of extreme values.
```python
from scipy.stats import trim_mean
trim_mean(state['Population'], 0.1)
# trim=0.1 drops 10% from each end
```
#### Robust (resistant)
Not sensitive to extreme values.

#### Outlier (extreme value)
A data value that is very different from most of the data.

## Estimates of Variability

#### Deviation (errors, residuals)
The difference between the observed values and the estimate of location.

#### Variance (of a sample) (mean-squared-error)
The sum of squared deviations from the mean divided by n – 1 where n is the number of data values.

#### Standard deviation
The square root of the variance.
```python
df[column].std()
```

#### Mean absolute deviation (l1-norm, Manhattan norm)
The mean of the absolute values of the deviations from the mean.

#### Median absolute deviation from the median (Robust)
The median of the absolute values of the deviations from the median.
Median absolute deviation = Median(|x1-m|, |x2-m|,..., |xN-m|)
```python
from statsmodels import robust
robust.scale.mad(df[column])
```
#### Range
The difference between the largest and the smallest value in a data set.

#### Order statistics (ranks)
Metrics based on the data values sorted from smallest to biggest.

#### Percentile (quantile)
The value such that P percent of the values take on this value or less and (100–P) percent take on this value or more.

```python
percentages = [0.05, 0.25, 0.5, 0.75, 0.95]
df = pd.DataFrame(df[column].quantile(percentages))
df.index = [f'{p * 100}%' for p in percentages]
# Format index by list comprehensions
df.T
```

#### Interquartile range
The difference between the 75th percentile and the 25th percentile.
```python
df[column].quantile(0.75) - df[column].quantile(0.25)
```

#### Degrees of Freedom (Not a concern for data scientist)
> The premise is that you want to make estimates about a population based on a sample. Find number of constraints in computing an estimate. Since the standard deviation depends on the calculation of sample mean, there is 1 constraints and there are n-1 degree of freedom. Therefore n-1 should be used instead of n

## Exploring data distribution

#### Boxplot (box and whiskers plot)
A plot introduced by Tukey as a quick way to visualize the distribution of data.
```python
import matplotlib.pylab as plt

ax = ((df[column]/1_000_000)
      .plot
      .box(figsize=(3, 4)))

# to separate the values of zero, we can use underscore
ax.set_ylabel('y_label')

plt.tight_layout()
#tight_layout automatically adjusts subplot params so that the subplot(s) 
#fits in to the figure area.abels, and titles.
plt.show()
```

#### Frequency table
A tally of the count of numeric data values that fall into a set of intervals (bins). A tabular version of the frequency counts found in a histogram.
Converts numeric data to an ordered factor 
```python
# Simple way to get
pd.cut(df[column], {desired binning}).value_counts()
```
#### Histogram
A plot of the frequency table with the bins on the x-axis and the count (or pro‐ portion) on the y-axis. While visually similar, bar charts should not be confused with histograms. 

```python
import matplotlib.pylab as plt

ax = ((df[column] / 1_000_000)
      .plot
      .hist(figsize=(4, 4), 
            bins = 10)
     )
ax.set_xlabel('desired x-axis')

plt.tight_layout()
plt.show()
```
#### Density plot
A smoothed version of the histogram, often based on a kernel density estimate.
```python
ax = df[column].plot.hist(density=True, xlim=[0,12], bins=range(1,12)) 
# density argument required to be True to add a kde plot below
df[column].plot.density(ax=ax)
ax.set_xlabel('Desired x-column')
```
## Exploring Binary and Categorical data

#### Mode
The most commonly occurring category or value in a data set.

#### Expected value
When the categories can be associated with a numeric value, this gives an average value based on a category’s probability of occurrence.

#### Bar charts
The frequency or proportion for each category plotted as bars.
```python
ax = df.transpose().plot.bar(figsize=(4, 4), legend=False)
ax.set_xlabel('x_axis')
ax.set_ylabel('y_axis')
```
#### Pie charts (less informative than bar chart, though have some business uses)
The frequency or proportion for each category plotted as wedges in a pie.

#### Difference between bar charts and histogram
> In a bar chart the x-axis represents different categories of a factor variable, while in a histogram the x-axis represents values of a single variable on a numeric scale. In a histogram, the bars are typically shown touching each other, with gaps indicating values that did not occur in the data. In a bar chart, the bars are shown separate from one another.

## Correlation
Variables X and Y (each with measured data) are said to be posi‐ tively correlated if high values of X go with high values of Y, and low values of X go with low values of Y. If high values of X go with low values of Y, and vice versa, the variables are negatively correlated.

#### Correlation coefficient (Pearson)
A metric that measures the extent to which numeric variables are associated with one another (ranges from –1 to +1).

#### Correlation matrix
A table where the variables are shown on both rows and columns, and the cell values are the correlations between the variables.

```python
import matplotlib.pylab as plt

fig, ax = plt.subplots(figsize=(5, 4))
ax = sns.heatmap(etfs.corr(), vmin=-1, vmax=1, 
                 cmap=sns.diverging_palette(20, 220, as_cmap=True),
                 ax=ax)

plt.tight_layout()
plt.show()
```

##### Ellipse plot

```python
from matplotlib.collections import EllipseCollection
from matplotlib.colors import Normalize

def plot_corr_ellipses(data, figsize=None, **kwargs):
    ''' https://stackoverflow.com/a/34558488 '''
    M = np.array(data)
    if not M.ndim == 2:
        raise ValueError('data must be a 2D array')
    fig, ax = plt.subplots(1, 1, figsize=figsize, subplot_kw={'aspect':'equal'})
    ax.set_xlim(-0.5, M.shape[1] - 0.5)
    ax.set_ylim(-0.5, M.shape[0] - 0.5)
    ax.invert_yaxis()

    # xy locations of each ellipse center
    xy = np.indices(M.shape)[::-1].reshape(2, -1).T

    # set the relative sizes of the major/minor axes according to the strength of
    # the positive/negative correlation
    w = np.ones_like(M).ravel() + 0.01
    h = 1 - np.abs(M).ravel() - 0.01
    a = 45 * np.sign(M).ravel()

    ec = EllipseCollection(widths=w, heights=h, angles=a, units='x', offsets=xy,
                           norm=Normalize(vmin=-1, vmax=1),
                           transOffset=ax.transData, array=M.ravel(), **kwargs)
    ax.add_collection(ec)

    # if data is a DataFrame, use the row/column names as tick labels
    if isinstance(data, pd.DataFrame):
        ax.set_xticks(np.arange(M.shape[1]))
        ax.set_xticklabels(data.columns, rotation=90)
        ax.set_yticks(np.arange(M.shape[0]))
        ax.set_yticklabels(data.index)

    return ec
    
m = plot_corr_ellipses(etfs.corr(), figsize=(5, 4), cmap='bwr_r')
cb = fig.colorbar(m)
cb.set_label('Correlation coefficient')

plt.tight_layout()
plt.show()
```

#### Scatterplot
A plot in which the x-axis is the value of one variable, and the y-axis the value of another.


