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



