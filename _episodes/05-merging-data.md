---
title: Combining DataFrames with Pandas
objectives:
- Combine data from multiple files into a single DataFrame using merge and concat.
- Combine two DataFrames using a unique ID found in both DataFrames.
- Employ to_csv to export a DataFrame in CSV format.
- Join DataFrames using common fields (join keys).
questions:
- Can I work with data from multiple sources?
- How can I combine data from different data sets?
keypoints:
- Pandas' merge and concat can be used to combine subsets of a DataFrame, or even data from different files.
- join function combines DataFrames based on index or column.
- Joining two DataFrames can be done in multiple ways (left, right, and inner) depending on what data must be in the final DataFrame.
- to_csv can be used to write out DataFrames in CSV format.
---


In many "real world" situations, the data that we want to use come in multiple
files. We often need to combine these files into a single DataFrame to analyze
the data. The pandas package provides [various methods for combining
DataFrames](https://pandas.pydata.org/pandas-docs/stable/user_guide/merging.html) including
merge and concat.

To work through the examples below, we first need to load the species and
surveys files into pandas DataFrames. In a Jupyter Notebook or iPython:

```python
import pandas as pd
new_births_df = pd.read_csv("data/US_births_2000-2014_SSA.csv",
                         keep_default_na=False, na_values=[""])
new_births_df
```

![New Births](/assets/img/new-births.png)

```python
old_births_df = pd.read_csv("data/US_births_1994-2003_CDC_NCHS.csv",
                         keep_default_na=False, na_values=[""])
species_df
```

![Old Births](/assets/img/old-births.png)


Take note that the read_csv method we used can take some additional options which
we didn't use previously. Many functions in Python have a set of options that
can be set by the user if needed. In this case, we have told pandas to assign
empty values in our CSV to NaN with the parameters keep_default_na=False and na_values=[""].
We have explicitly requested to change empty values in the CSV to NaN,
this is however also the default behaviour of read_csv.
[More about all of the read_csv options here and their defaults.](https://pandas.pydata.org/pandas-docs/stable/reference/api/pandas.read_csv.html#pandas.read_csv)

## Concatenating DataFrames

We can use the concat function in pandas to append either columns or rows from
one DataFrame to another.  Let's grab two subsets of our data to see how this
works.

```python
# Read in first 10 lines of surveys table
new_sub = new_births_df.head(10)
# Grab the last 10 rows
new_sub_last10 = new_births_df.tail(10)
# Reset the index values to the second dataframe appends properly
new_sub_last10 = new_sub_last10.reset_index(drop=True)
# drop=True option avoids adding new index column with old index values
```

When we concatenate DataFrames, we need to specify the axis. axis=0 tells
pandas to stack the second DataFrame UNDER the first one. It will automatically
detect whether the column names are the same and will stack accordingly.
axis=1 will stack the columns in the second DataFrame to the RIGHT of the
first DataFrame. To stack the data vertically, we need to make sure we have the
same columns and associated column format in both datasets. When we stack
horizontally, we want to make sure what we are doing makes sense (i.e. the data are
related in some way).

```python
# Stack the DataFrames on top of each other
vertical_stack = pd.concat([survey_sub, survey_sub_last10], axis=0)

# Place the DataFrames side by side
horizontal_stack = pd.concat([survey_sub, survey_sub_last10], axis=1)
```

#### Row Index Values and Concat

Have a look at the vertical_stack DataFrame. Notice anything unusual?
The row indexes for the two DataFrames survey_sub and survey_sub_last10
have been repeated. We can reindex the new DataFrame using the reset_index() method.

### Writing Out Data to CSV

We can use the to_csv command to export a DataFrame in CSV format. Note that the code
below will by default save the data into the current working directory. We can
save it to a different folder by adding the foldername and a slash to the file
vertical_stack.to_csv('foldername/out.csv'). We use the index=False so that
pandas doesn't include the index number for each line.

```python
# Write DataFrame to CSV
vertical_stack.to_csv('data/out.csv', index=False)
```

Check out your working directory to make sure the CSV wrote out properly, and
that you can open it! If you want, try to bring it back into pandas to make sure
it imports properly.

```python
# For kicks read our output back into Python and make sure all looks good
new_output = pd.read_csv('data/out.csv', keep_default_na=False, na_values=[""])
```

:::::::::::::::::::::::::::::::::::::::  challenge

> ## Challenge - Combine Data
> Read the data from the two files,
> US_births_2000-2014_SSA.csv and US_births_1994-2003_CDC_NCHS.csv,
> into pandas and combine the files to make one new DataFrame.
> 
> Create a plot of births by year, grouped by month.
> Export your results as a CSV and make sure it reads back into pandas properly.
> 
> > ## Solution
> > 
> > ```python
> > # read the files:
> > births1 = pd.read_csv("data/US_births_1994-2003_CDC_NCHS.csv")
> > births2 = pd.read_csv("data/US_births_2000-2014_SSA.csv")
> > # concatenate
> > births_all = pd.concat([births1, births2], axis=0)
> > # get the birth for each year:
> > birth_year = births_all.groupby(['year']).mean()["births"].unstack()
> > # plot:
> > birth_year.plot(kind="bar")
> > plot.tight_layout()  # tip: use this to improve the plot layout. 
> > # Try running the code without this line to see 
> > # what difference applying plt.tight_layout() makes.
> > ```
> > 
> > ![Birth Year Plot](birth-year-plot.png){alt='average birth for each year}
> > 
> > ```python
> > # writing to file:
> > birth_year.to_csv("births_for_year.csv")
> > # reading it back in:
> > pd.read_csv("births_for_year.csv", index_col=0)
> > ```
> > 
> {: .solution}
{: .challenge}


