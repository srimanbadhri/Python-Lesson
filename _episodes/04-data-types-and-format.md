---
title: Data Types and Formats
objectives:
- Describe how information is stored in a pandas DataFrame.
- Define the two main types of data in pandas: text and numerics.
- Examine the structure of a DataFrame.
- Modify the format of values in a DataFrame.
- Describe how data types impact operations.
- Define, manipulate, and interconvert integers and floats in Python/pandas.
- Analyze datasets having missing/null values (NaN values).
- Write manipulated data to a file.
questions:
- What types of data can be contained in a DataFrame?
- Why is the data type important?
keypoints:

---



The format of individual columns and rows will impact analysis performed on a
dataset read into a pandas DataFrame. For example, you can't perform mathematical
calculations on a string (text formatted data). This might seem obvious,
however sometimes numeric values are read into pandas as strings. In this
situation, when you then try to perform calculations on the string-formatted
numeric data, you get an error.

In this lesson we will review ways to explore and better understand the
structure and format of our data.

## Types of Data

How information is stored in a
DataFrame or a Python object affects what we can do with it and the outputs of
calculations as well. There are two main types of data that we will explore in
this lesson: numeric and text data types.

## Numeric Data Types

Numeric data types include integers and floats. A **floating point** (known as a
float) number has decimal points even if that decimal point value is 0. For
example: 1.13, 2.0, 1234.345. If we have a column that contains both integers and
floating point numbers, pandas will assign the entire column to the float data
type so the decimal points are not lost.

An **integer** will never have a decimal point. Thus if we wanted to store 1.13 as
an integer it would be stored as 1. Similarly, 1234.345 would be stored as 1234. You
will often see the data type Int64 in pandas which stands for 64 bit integer. The 64
refers to the memory allocated to store data in each cell which effectively
relates to how many digits it can store in each "cell". Allocating space ahead of time
allows computers to optimize storage and processing efficiency.

## Text Data Type

The text data type is known as a *string* in Python, or *object* in pandas. Strings can
contain numbers and / or characters. For example, a string might be a word, a
sentence, or several sentences. A pandas object might also be a plot name like
'plot1'. A string can also contain or consist of numbers. For instance, '1234'
could be stored as a string, as could '10.23'. However **strings that contain
numbers can not be used for mathematical operations**!

pandas and base Python use slightly different names for data types. More on this
is in the table below:

| Pandas Type           | Native Python Type                    | Description                                                                                                                                                    | 
| --------------------- | ------------------------------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| object                | string                                | The most general dtype. Will be assigned to your column if column has mixed types (numbers and strings).                                                       | 
| int64                 | int                                   | Numeric characters. 64 refers to the memory allocated to hold this character.                                                                                  | 
| float64               | float                                 | Numeric characters with decimals. If a column contains numbers and NaNs (see below), pandas will default to float64, in case your missing value has a decimal. | 
| datetime64, timedelta[ns] | N/A (but see the [datetime]("https://doc.python.org/2/library/datetime.html") module in Python's standard library)                     | Values meant to hold time data. Look into these for time series experiments.                                                                                   | 

## Checking the format of our data

Now that we're armed with a basic understanding of numeric and text data
types, let's explore the format of our survey data. We'll be working with the
same dataset that we've used in previous lessons.

```python
# Make sure pandas is loaded
import pandas as pd

# Note that pd.read_csv is used because we imported pandas as pd
births_df = pd.read_csv("data/US_births_2000-2014_SSA.csv")
```

Remember that we can check the type of an object like this:

```python
type(births_df)
```

```output
pandas.core.frame.DataFrame
```

Next, let's look at the structure of our births_df data. In pandas, we can check
the type of one column in a DataFrame using the syntax
dataframe_name['column_name'].dtype:


(text).

```python
births_df['year'].dtype
```

```output
dtype('int64')
```

The type int64 tells us that pandas is storing each value within this column
as a 64 bit integer. We can use the dataframe_name.dtypes command to view the data type
for each column in a DataFrame (all at once).

```python
births_df.dtypes
```

which returns:

```python 
year             int64
month            int64
date_of_month    int64
day_of_week      int64
births           int64
dtype: object
```

Note that most of the columns in our births_df data are of type int64. This means that they are 64 bit integers.

## Working With Integers and Floats

So we've learned that computers store numbers in one of two ways: as integers or
as floating-point numbers (or floats). Integers are the numbers we usually count
with. Floats have fractional parts (decimal places). Let's next consider how
the data type can impact mathematical operations on our data. Addition,
subtraction, division and multiplication work on floats and integers as we'd expect.

```python
print(5+5)
```

```output
10
```

```python
print(24-4)
```

```output
20
```

If we divide one integer by another, we get a float.
The result on Python 3 is different than in Python 2, where the result is an
integer (integer division).

```python
print(5/9)
```

```output
0.5555555555555556
```

```python
print(10/3)
```

```output
3.3333333333333335
```

We can also convert a floating point number to an integer or an integer to
floating point number. Notice that Python by default rounds down when it
converts from floating point to integer.

```python
# Convert a to an integer
a = 7.83
int(a)
```

```output
7
```

```python
# Convert b to a float
b = 7
float(b)
```

```output
7.0
```

## Working With Our Births Data

Getting back to our data, we can modify the format of values within our data, if
we want. For instance, we could convert the year field to floating point
values.

```python
# Convert the record_id field from an integer to a float
births_df['year'] = births_df['year'].astype('float64')
births_df['year'].dtype
```

```output
dtype('float64')
```

> ## Challenge - Changing Types
>
> Try converting the column years to floats using
>
> ```python
> births_df['births'].astype("float")
> ```
> 
> Next try converting years to an integer.
>
> > ## Solution
> > 
> > ```python
> > births_df['births'].astype("float")
> > ```
> > 
> > ```output
> > 0        9083.0
> > 1        8006.0
> > 2       11363.0
> > 3       13032.0
> > 4       12558.0
> >          ...   
> > 5474     8656.0
> > 5475     7724.0
> > 5476    12811.0
> > 5477    13634.0
> > 5478    11990.0
> > Name: births, Length: 5479, dtype: float64
> > ```
> > 
> > ```python
> > births_df['births'].astype("int")
> > ```
> > 
> > ```output
> > 0        9083
> > 1        8006
> > 2       11363
> > 3       13032
> > 4       12558
> >         ...  
> > 5474     8656
> > 5475     7724
> > 5476    12811
> > 5477    13634
> > 5478    11990
> > Name: births, Length: 5479, dtype: int64
> > ```
> > 
> {: .solution}
{: .challenge}


## Writing Out Data to CSV

We've learned about manipulating data to get desired outputs. But we've also discussed
keeping data that has been manipulated separate from our raw data. Something we might be interested
in doing is working with only the columns that have full data. First, let's reload the data so
we're not mixing up all of our previous manipulations.

```python
births = pd.read_csv("data/US_births_2000-2014_SSA.csv")
```

Next, let's drop all the rows that contain missing values. We will use the command dropna.
By default, dropna removes rows that contain missing data for even just one column.

```python
df_na = births_df.dropna()
```

If you now type df_na, you should observe that the resulting DataFrame has 5479 rows
and 5 columns

We can now use the to_csv command to export a DataFrame in CSV format. Note that the code
below will by default save the data into the current working directory. We can
save it to a different folder by adding the foldername and a slash before the filename:
df.to_csv('foldername/out.csv'). We use 'index=False' so that
pandas doesn't include the index number for each line.

```python
# Write DataFrame to CSV
df_na.to_csv('data_output/births_complete.csv', index=False)
```

We will use this data file later in the workshop. Check out your working directory to make
sure the CSV wrote out properly, and that you can open it! If you want, try to bring it
back into Python to make sure it imports properly.


> ## Instructor Note - Processed Data Checkpoint:
>
> If learners have trouble generating the output, or anything happens with that, the folder
> [sample_output](https://github.com/datacarpentry/python-ecology-lesson/tree/main/sample_output)
> in this repository contains the file surveys_complete.csv with the data they should generate.
> 
> {: .source}
{: .callout}


## Recap

What we've learned:

- How to explore the data types of columns within a DataFrame
- How to change the data type
- What NaN values are, how they might be represented, and what this means for your work
- How to replace NaN values, if desired
- How to use to_csv to write manipulated data to a file.


