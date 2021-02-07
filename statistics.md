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

