---
title: Indexing, Slicing and Subsetting DataFrames in Python
objectives:
- Describe what 0-based indexing is.
- Manipulate and extract data using column headings and index locations.
- Employ slicing to select sets of data from a DataFrame.
- Employ label and integer-based indexing to select ranges of data in a dataframe.
- Reassign values within subsets of a DataFrame.
- Create a copy of a DataFrame.
- Query / select a subset of data using a set of criteria using the following operators: ==, !=, >, <, >=, <=.
- Locate subsets of data using masks.
- Describe BOOLEAN objects in Python and manipulate data using BOOLEANs.
questions:
- How can I access specific data within my data set?
- How can Python and Pandas help me to analyse my data?
keypoints:
- In Python, portions of data can be accessed using indices, slices, column headings, and condition-based subsetting.
- Python uses 0-based indexing, in which the first element in a list, tuple or any other data structure has an index of 0.
- Pandas enables common data exploration steps such as data indexing, slicing and conditional subsetting.

---

> ## Instructor Note
> Tip: use .head() method throughout this lesson to keep your display 
> neater for students.
> Encourage students to try with and without .head() 
> to reinforce this useful tool and then to use it or not at their 
> preference.
> 
> For example, if a student worries about keeping up in pace with typing,
> let them know they can skip the .head(),
> but that you'll use it to keep more lines of previous steps visible.
> 
> {: .source}
{: .callout}


In the first episode of this lesson, we read a CSV file into a pandas' DataFrame. We learned how to:

- save a DataFrame to a named object,
- perform basic math on data,
- calculate summary statistics, and
- create plots based on the data we loaded into pandas.

In this lesson, we will explore ways to access different parts of the data using:

- indexing,
- slicing, and
- subsetting.

## Loading our data

We will continue to use the surveys dataset that we worked with in the last
episode. Let's reopen and read in the data again:

```python
# Make sure pandas is loaded
import pandas as pd

# Read in the survey CSV
pd.read_csv("data/US_births_2000-2014_SSA.csv")
```

## Indexing and Slicing in Python

We often want to work with subsets of a **DataFrame** object. There are
different ways to accomplish this including: using labels (column headings),
numeric ranges, or specific x,y index locations.

## Selecting data using Labels (Column Headings)

We use square brackets [] to select a subset of a Python object. For example,
we can select all data from a column named species_id from the births_df
DataFrame by name. There are two ways to do this:

```python
# TIP: use the .head() method we saw earlier to make output shorter
# Method 1: select a 'subset' of the data using the column name
births_df['year']

# Method 2: use the column name as an 'attribute'; gives the same output
births_df.year
```

We can also create a new object that contains only the data within the
species_id column as follows:

```python
# Creates an object, surveys_species, that only contains the species_id column
surveys_years = births_df['year']
```

We can pass a list of column names too, as an index to select columns in that
order. This is useful when we need to reorganize our data.

**NOTE:** If a column name is not contained in the DataFrame, an exception
(error) will be raised.

```python
# Select the species and plot columns from the DataFrame
births_df[['year', 'births']]

# What happens when you flip the order?
births_df[['births', 'year']]

# What happens if you ask for a column that doesn't exist?
births_df['fake_column']
```

Python tells us what type of error it is in the traceback, at the bottom it says
KeyError: 'fake_column' which means that fake_column is not a valid column name (nor a valid key in
the related Python data type dictionary).


> ## Reminder
> The Python language and its modules (such as Pandas) define reserved
words that should not be used as identifiers when assigning objects
and variable names. Examples of reserved words in Python include Boolean values
> True and False, operators and, or, and not, among others. The full list
of reserved words for Python version 3 is provided at
[https://docs.python.org/3/reference/lexical\_analysis.html#identifiers](https://docs.python.org/3/reference/lexical_analysis.html#identifiers).
>
> When naming objects and variables, it's also important to avoid using
the names of built-in data structures and methods. For example, a *list* is a built-in
data type. It is possible to use the word 'list' as an identifier for a new object,
for example list = ['apples', 'oranges', 'bananas']. However, you would then
be unable to create an empty list using list() or convert a tuple to a
list using list(sometuple).
>
> {: .source}
{: .callout}

> ## Reminder
> 
> Python uses 0-based indexing.
> 
> {: .source}
{: .callout}

## Extracting Range based Subsets: Slicing

Let's remind ourselves that Python uses 0-based
indexing. This means that the first element in an object is located at position
0. This is different from other tools like R and Matlab that index elements
within objects starting at 1.

```python
# Create a list of numbers:
a = [1, 2, 3, 4, 5]
```

![Indexing Diagram](/assets/img/slicing-indexing.png)
![Slicing Diagram](/assets/img/slicing-slicing.png)


> ## Challenge - Extracting Data
> 
> 1. What value does the code below return?
>   ```python 
>   a[0]
>   ```
>2. How about this:
>   ```python 
>   a[5]
>   ```
>3. In the example above, calling a[5] returns an error. Why is that?
>4. What about?
>   ```python 
>   a[len(a)]
>   ```
> 
> > ## Solution
> > 
> > 1. a[0] returns 1, as  Python starts with element 0 
> > (this may be different from what you have previously experience with other 
> > languages e.g. MATLAB and R)
> > 2. a[5] raises an IndexError
> > 3. The error is raised because the list a has no element with index 5:
> > it has only five entries, indexed from 0 to 4.
> > 4. a[len(a)] also raises an IndexError
> >    len(a) returns 5, making a[len(a)] equivalent to a[5].
> >    To retreive the final element of a list, us the index -1, e.g.
> > 
> >    ```python
> >    a[-1]
> >    ```
> >    
> >    ```output
> >    5
> >    ```
> > 
> {: .solution}
{: .challenge}



## Slicing Subsets of Rows in Python

Slicing using the [] operator selects a set of rows and/or columns from a
DataFrame. To slice out a set of rows, you use the following syntax:
data[start:stop]. When slicing in pandas the start bound is included in the
output. The stop bound is one step BEYOND the row you want to select. So if you
want to select rows 0, 1 and 2 your code would look like this:

```python
# Select rows 0, 1, 2 (row 3 is not selected)
births_df[0:3]
```

The stop bound in Python is different from what you might be used to in
languages like Matlab and R.

```python
# Select the first 5 rows (rows 0, 1, 2, 3, 4)
births_df[:5]

# Select the last element in the list
# (the slice starts at the last element, and ends at the end of the list)
births_df[-1:]
```

We can also reassign values within subsets of our DataFrame.

But before we do that, let's look at the difference between the concept of
copying objects and the concept of referencing objects in Python.

## Copying Objects vs Referencing Objects in Python

Let's start with an example:

```python
# Using the 'copy() method'
true_copy_births_df = births_df.copy()

# Using the '=' operator
ref_births_df = births_df
```

You might think that the code ref_births_df = births_df creates a fresh
distinct copy of the births_df DataFrame object. However, using the =
operator in the simple statement y = x does **not** create a copy of our
DataFrame. Instead, y = x creates a new variable y that references the
**same** object that x refers to. To state this another way, there is only
**one** object (the DataFrame), and both x and y refer to it.

In contrast, the copy() method for a DataFrame creates a true copy of the
DataFrame.

Let's look at what happens when we reassign the values within a subset of the
DataFrame that references another DataFrame object:

```python
# Assign the value 0 to the first three rows of data in the DataFrame
ref_births_df[0:3] = 0
```

Let's try the following code:

```python
# ref_births_df was created using the '=' operator
ref_births_df.head()

# true_copy_births_df was created using the copy() function
true_copy_births_df.head()

# births_df is the original dataframe
births_df.head()
```

What is the difference between these three dataframes?

When we assigned the first 3 rows the value of 0 using the
ref_births_df DataFrame, the births_df DataFrame is modified too.
Remember we created the reference ref_births_df object above when we did
ref_births_df = births_df. Remember births_df and ref_births_df
refer to the same exact DataFrame object. If either one changes the object,
the other will see the same changes to the reference object.

However - true_copy_births_df was created via the copy() function.
It retains the original values for the first three rows.

**To review and recap**:

- **Copy** uses the dataframe's copy() method
  
  ```python 
  true_copy_births_df = births_df.copy()
  ```

- A **Reference** is created using the = operator
  
  ```python 
  ref_births_df = births_df
  ```
Okay, that's enough of that. Let's create a brand new clean dataframe from
the original data CSV file.

```python
births_df = pd.read_csv("data/US_births_2000-2014_SSA.csv")
```

## Slicing Subsets of Rows and Columns in Python

We can select specific ranges of our data in both the row and column directions
using either label or integer-based indexing.

- iloc is primarily an *integer* based indexing counting from 0. That is, your
  specify rows and columns giving a number. Thus, the first row is row 0,
  the second column is column 1, etc.

- loc is primarily a *label* based indexing where you can refer to rows and
  columns by their name. E.g., column 'month'. Note that *integers* may be
  used, but they are interpreted as a *label*.

To select a subset of rows **and** columns from our DataFrame, we can use the
iloc method. For example, we can select month, day and year (columns 2, 3
and 4 if we start counting at 1), like this:

```python
# iloc[row slicing, column slicing]
births_df.iloc[0:3, 1:4]
```

which gives the **output**

![Small Slice](/assets/img/small-slice.png)


Notice that we asked for a slice from 0:3. This yielded 3 rows of data. When you
ask for 0:3, you are actually telling Python to start at index 0 and select rows
0, 1, 2 **up to but not including 3**.

Let's explore some other ways to index and select subsets of data:

```python
# Select all columns for rows of index values 0 and 10
births_df.loc[[0, 10], :]

# What does this do?
births_df.loc[0, ['year', 'month', 'date_of_month']]

# What happens when you type the code below?
births_df.loc[[0, 10, 35549], :]
```

**NOTE**: Labels must be found in the DataFrame or you will get a KeyError.

Indexing by labels loc differs from indexing by integers iloc.
With loc, both the start bound and the stop bound are **inclusive**. When using
loc, integers *can* be used, but the integers refer to the
index label and not the position. For example, using loc and select 1:4
will get a different result than using iloc to select rows 1:4.

We can also select a specific data value using a row and
column location within the DataFrame and iloc indexing:

```python
# Syntax for iloc indexing to finding a specific data element
dat.iloc[row, column]
```

In this iloc example,

```python
births_df.iloc[2, 3]
```

gives the **output**

```output
'1'
```

Remember that Python indexing begins at 0. So, the index location [2, 6]
selects the element that is 3 rows down and 7 columns over in the DataFrame.

It is worth noting that rows are selected when using loc with a single list of
labels (or iloc with a single list of integers). However, unlike loc or iloc,
indexing a data frame directly with labels will select columns (e.g.
births_df[['species_id', 'plot_id', 'weight']]), while ranges of integers will
select rows (e.g. surveys\_df[0:13]). Direct indexing of rows is redundant with
using iloc, and will raise a KeyError if a single integer or list is used; the
error will also occur if index labels are used without loc (or column labels used
with it).
A useful rule of thumb is the following: integer-based slicing is best done with
iloc and will avoid errors (and is generally consistent with indexing of Numpy
arrays), label-based slicing of rows is done with loc, and slicing of columns by
directly indexing column names.


> ## Challenge - Range
> 
> 1. What happens when you execute:
>    - births_df[0:1]
>    - births_df[0]
>    - births_df[:4]
>    - births_df[:-1]
> 2. What happens when you call:
>    - births_df.iloc[0:1]
>    - births_df.iloc[0]
>    - births_df.iloc[:4, :]
>    - births_df.iloc[0:4, 1:4]
>    - births_df.loc[0:4, 1:4]
> 3. How are the last two commands different?
> 
> > ## Solution
> > 
> > 1. 
> >    - births_df[0:3] returns the first three rows of the DataFrame:
> >    ![Top 3](/assets/img/top-three.png)
> >    - births_df[0:1] can be used to obtain only the first row.
> >    - births_df[0] results in a 'KeyError', since direct indexing of a row is redundant with iloc.
> >    - births_df[:4] slices from the first row to the fourth:
> >    ![Slice To 4](/assets/img/four-slice.png)
> >    - births_df[:-1] provides everything except the final row of the DataFrame.
> >     You can use negative index numbers to count backwards from the last entry.
> > 2. 
> >    - births_df.iloc[0:1] returns the first row
> >    - births_df.iloc[0] returns the first row as a named list
> >    - births_df.iloc[:4, :] returns all columns of the first four rows
> >    - births_df.iloc[0:4, 1:4] selects specified columns of the first four rows
> >    - births_df.loc[0:4, 1:4] results in a 'TypeError' - see below.
> > 3. While iloc uses integers as indices and slices accordingly, loc works with labels.
> >    It is like accessing values from a dictionary, asking for the key names. 
> >    Column names 1:4 do not exist, so the call to loc above results in an error. 
> >    Check also the difference between births_df.loc[0:4] and births_df.iloc[0:4].
> > 
> {: .solution}
{: .challenge}


## Subsetting Data using Criteria

We can also select a subset of our data using criteria. For example, we can
select all rows that have a year value of 2002:

```python
births_df[births_df.year == 2002]
```

Which produces the following output:

![2002 Data](/assets/img/2002-data.png)


Or we can select all rows that do not contain the year 2002:

```python
births_df[births_df.year != 2002]
```

We can define sets of criteria too:

```python
births_df[(births_df.year >= 1980) & (births_df.year <= 1985)]
```

### Python Syntax Cheat Sheet

We can use the syntax below when querying data by criteria from a DataFrame.
Experiment with selecting various subsets of the "surveys" data.

- Equals: ==
- Not equals: !=
- Greater than: >
- Less than: <
- Greater than or equal to: >=
- Less than or equal to: <=



> ## Challenge - Queries
> 
> 1. Select a subset of rows in the births_df DataFrame that contain 
>    data from the February 2012
>    How many rows did you end up with? What did your neighbor get?
> 2. Experiment with other queries. 
>    Create a query that finds all rows with 
>    a births value greater than or equal to 10,000.
> 3. The ~ symbol in Python can be used to return the OPPOSITE of the
>    selection that you specify.
>    It is equivalent to **is not in**.
>    Write a query that selects all rows with date_of_week NOT equal to 1-5 in the data.
> 
> > ## Solution
> > 
> > 1:
> >    ```python
> >    births_df[(births_df["year"] == 2012) & (births_df["month"] == 2)]
> >    ```
> > ![Feb 2012 Data](/assets/img/feb-2012-data.png)
> > 
> >    If you are only interested in how many rows meet the criteria, 
> >    the sum of True values could be used instead:
> >    
> >    ```python
> >    sum((births_df["year"] == 2012) & (births_df["month"] == 2))
> >    ```
> >    
> >    ```output
> >    29
> >    ```
> > 2: births_df[births_df["births"] >= 10000]
> > 
> > 3: 
> >    ```python
> >    births_df[~births_df["day_of_week"].isin([1, 2, 3, 4, 5])]
> >    ```
> > ![1-5 Weekday Data](/assets/img/no-weekend-data.png)
> > 
> {: .solution}
{: .challenge}


> ## Instructor Note:
>
> When working through the solutions to the challenges above,
> you could introduce already that all these slice operations are actually based on a
> *Boolean indexing* operation (next section in the lesson).
> The filter provides for each record if it satisfies (True) or not (False).
> The slicing itself interprets the True/False of each record.
> 
> {: .source}
{: .callout}


## Using masks to identify a specific condition

A **mask** can be useful to locate where a particular subset of values exist or
don't exist - for example,  NaN, or "Not a Number" values. To understand masks,
we also need to understand BOOLEAN objects in Python.

Boolean values include True or False. For example,

```python
# Set x to 5
x = 5

# What does the code below return?
x > 5

# How about this?
x == 5
```

When we ask Python whether x is greater than 5, it returns False.
This is Python's way to say "No". Indeed, the value of x is 5,
and 5 is not greater than 5.

To create a boolean mask:

- Set the True / False criteria (e.g. values > 5 = True)
- Python will then assess each value in the object to determine whether the
  value meets the criteria (True) or not (False).
- Python creates an output object that is the same shape as the original
  object, but with a True or False value for each index location.

Let's try this out. Let's identify all locations in the survey data that have
null (missing or NaN) data values. We can use the isnull method to do this.
The isnull method will compare each cell with a null value. If an element
has a null value, it will be assigned a value of True in the output object.

```python
pd.isnull(births_df)
```

A snippet of the output is below:
> > ![All False](/assets/img/all-false.png)



To select the rows where there are null values, we can use
the mask as an index to subset our data as follows:

```python
# To select just the rows with NaN values, we can use the 'any()' method
births_df[pd.isnull(births_df).any(axis=1)]
```

Note that the weight column of our DataFrame contains many null or NaN
values. We will explore ways of dealing with this in the next episode on [Data Types and Formats](04-data-types-and-format.md).

We can run isnull on a particular column too. What does the code below do?

```python
# What does this do?
empty_weights = births_df[pd.isnull(births_df['weight'])]['weight']
print(empty_weights)
```

Let's take a minute to look at the statement above. We are using the Boolean
object pd.isnull(births_df['weight']) as an index to births_df. We are
asking Python to select rows that have a NaN value of weight.

> ## Challenge - Putting It All Together
> 
> 1. Create a new DataFrame that only contains observations with day_of_week values that are **not** 1-7
>    Print the number of rows in this new DataFrame.
>    Verify the result by comparing the number of rows in the new DataFrame with
>    the number of rows in the surveys DataFrame where day_of_week is null.
> 2. Create a new DataFrame that contains only observations that are of day_of_week 6
>    or 7 and where births values are greater than 10,000.
>    Create a stacked bar plot of average weight by plot 
>    with 6 vs 7 values stacked for each plot.
> 
> > ## Solution
> > 
> > 1:
> >    ```python
> >    new = births_df[~births_df['day_of_week'].isin([1, 2, 3, 4, 5, 6, 7])].copy()
> >    print(len(new))
> >    ```
> >    
> >    ```output
> >    0
> >    ```
> >    
> >    ```python
> >    sum(births_df['day_of_week'].isnull()) == len(new)
> >    ```
> >    
> >    ```output
> >    True
> >    ```
> > 
> > 2: 
> >    ```python
> >    # selection of the data with isin
> >    stack_selection = births_df[(births_df['day_of_week'].isin([6, 7])) & (births_df['births'] > 10000)]
> > 
> >    # calculate the mean weight for each plot id and sex combination:
> >    stack_selection = stack_selection.groupby(["month", "day_of_week"]).mean().unstack()
> >    # and we can make a stacked bar plot from this:
> >    stack_selection.plot(kind='bar', stacked=True)
> >    ```
> > 
> {: .solution}
{: .challenge}


> ## Instructor Note:
>
> Referring to the challenge solution above,
> as we know the other values are all Nan values, we could also select all not null
> values:
> ```python
> stack_selection = births_df[(births_df['sex'].notnull()) &
>           births_df["weight"] > 0.][["sex", "weight", "plot_id"]]
> ```
> However, due to the unstack command, the legend header contains two levels.
>In order to remove this, the column naming needs to be simplified:
> 
> ```python
stack_selection.columns = stack_selection.columns.droplevel()
> ```
> This is just a preview, more in next episode.
> 
> {: .source}
{: .callout}




