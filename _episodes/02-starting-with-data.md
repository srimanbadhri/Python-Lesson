---
title: Starting With Data
objectives:
- Navigate the workshop directory and download a dataset.
- Explain what a library is and what libraries are used for.
- Describe what the Python Data Analysis Library (Pandas) is.
- Load the Python Data Analysis Library (Pandas).
- Read tabular data into Python using Pandas.
- Describe what a DataFrame is in Python.
- Access and summarize data stored in a DataFrame.
- Define indexing as it relates to data structures.
- Perform basic mathematical operations and summary statistics on data in a Pandas DataFrame.
- Create simple plots.
questions:
- How can I import data in Python?
- What is Pandas?
- Why should I use Pandas to work with data?
keypoints:
- Libraries enable us to extend the functionality of Python.
- Pandas is a popular library for working with data.
- A Dataframe is a Pandas data structure that allows one to access data by column (name or index) or row.
- Aggregating data using the groupby() function enables you to generate useful summaries of data quickly.
- Plots can be created from DataFrames or subsets of data that have been generated with groupby().

---


## Working With Pandas DataFrames in Python

We can automate the process of performing data manipulations in Python. It's efficient to spend time
building the code to perform these tasks because once it's built, we can use it
over and over on different datasets that use a similar format. This makes our
data manipulation processes reproducible. We can also share our code with
colleagues and they can replicate the same analysis starting with the same original data.

#### Starting in the same spot

To help the lesson run smoothly, let's ensure everyone is in the same directory.
This should help us avoid path and file name issues. At this time please
navigate to the workshop directory. If you are working in Jupyter Notebook be sure
that you start your notebook in the workshop directory.

#### Our Data

For this lesson, we will be using the Portal Teaching data, a subset of the data
from Ernst et al.
[Long-term monitoring and experimental manipulation of a Chihuahuan Desert ecosystem near Portal,
Arizona, USA][ernst].

We will be using files from the [Portal Project Teaching Database](https://figshare.com/articles/Portal_Project_Teaching_Database/1314459).
This section will use the surveys.csv file that can be downloaded here:
[https://ndownloader.figshare.com/files/2292172](https://ndownloader.figshare.com/files/2292172)

We are studying the **species** and **weight** of animals caught in sites in our study
area. The dataset is stored as a .csv file: each row holds information for a
single birth, and the columns represent:

year,month,date_of_month,day_of_week,births


| Column          | Description                   | 
| --------------- | ----------------------------- |
| year            | year of observation           | 
| month           | month of observation          | 
| date\_of\_month  | day of observation           | 
| day\_of\_week   | weekday of observation        | 
| births          | # of births in that day       | 

The first few rows of our first file look like this:
![First Few Rows](/assets/img/first-few-rows.png)

***

### About Libraries

A library in Python contains a set of tools (called functions) that perform
tasks on our data. Importing a library is like getting a piece of lab equipment
out of a storage locker and setting it up on the bench for use in a project.
Once a library is set up, it can be used or called to perform the task(s)
it was built to do.

### Pandas in Python

One of the best options for working with tabular data in Python is to use the
[Python Data Analysis Library](https://pandas.pydata.org) (a.k.a. Pandas). The
Pandas library provides data structures, produces high quality plots with
[matplotlib] and integrates nicely with other libraries
that use [NumPy](https://www.numpy.org/) (which is another Python library) arrays.

Python doesn't load all of the libraries available to it by default. We have to
add an import statement to our code in order to use library functions. To import
a library, we use the syntax import libraryName. If we want to give the
library a nickname to shorten the command, we can add as nickNameHere.  An
example of importing the pandas library using the common nickname pd is below.

```python
import pandas as pd
```

Each time we call a function that's in a library, we use the syntax
LibraryName.FunctionName. Adding the library name with a . before the
function name tells Python where to find the function. In the example above, we
have imported Pandas as pd. This means we don't have to type out pandas each
time we call a Pandas function.

## Reading CSV Data Using Pandas

We will begin by locating and reading our survey data which are in CSV format. CSV stands for
Comma-Separated Values and is a common way to store formatted data. Other symbols may also be used, so
you might see tab-separated, colon-separated or space separated files. pandas can work with each of these
types of separators, as it allows you to specify the appropriate separator for your data.
CSV files (and other -separated value file types) make it easy to share data, and can be imported and exported
from many applications, including Microsoft Excel. For more details on CSV
files, see the [Data Organisation in Spreadsheets](https://www.datacarpentry.org/spreadsheet-ecology-lesson/05-exporting-data) lesson.
We can use Pandas' read_csv function to pull the file directly into a [DataFrame](https://pandas.pydata.org/docs/reference/api/pandas.DataFrame.html).


### So What's a DataFrame?

A DataFrame is a 2-dimensional data structure that can store data of different
types (including strings, numbers, categories and more)
in columns. It is similar to a spreadsheet or an SQL table or the data.frame in
R. A DataFrame always has an index (0-based). An index refers to the position of
an element in the data structure.

```python
# Note that pd.read_csv is used because we imported pandas as pd
pd.read_csv("data/US_births_2000-2014_SSA.csv")
```

The above command yields the **output** below:

![Read CSV](/assets/img/read-csv.png)


We can see that there were 5,479 rows parsed. Each row has 6
columns. The first column is the index of the DataFrame. The index is used to
identify the position of the data, but it is not an actual column of the DataFrame.
It looks like  the read_csv function in Pandas  read our file properly. However,
we haven't saved any data to memory so we can work with it. We need to assign the
DataFrame to a variable. Remember that a variable is a name for a value, such as x,
or  data. We can create a new  object with a variable name by assigning a value to it using =.

Let's call the imported data births_df:

```python
births_df = pd.read_csv("data/US_births_2000-2014_SSA.csv")
```

Note that Python does not produce any output on the screen  when you assign the imported DataFrame to a variable.
We can view the value of the births_df
object by typing its name into the Python command prompt.

```python
births_df
```

which prints contents like above.

Note: if the output is too wide to print on your narrow terminal window, you may see something
slightly different as the large set of data scrolls past. 
Don't worry: all the data is there! You can confirm this by scrolling upwards, or by
looking at the [# of rows x # of columns] block at the end of the output.

You can also use births_df.head() to view only the first few rows of the dataset in an output
that is easier to fit in one window. After doing this, you can see that pandas has neatly formatted
the data to fit our screen:

```python
births_df.head() # The head() method displays the first several lines of a file. It
                  # is discussed below.
```

![Survey DF Head](/assets/img/df-head.png)

### Exploring Our Species Survey Data

Again, we can use the type function to see what kind of thing births_df is:

```python
type(births_df)
```

```output
<class 'pandas.core.frame.DataFrame'>
```

As expected, it's a DataFrame (or, to use the full name that Python uses to refer
to it internally, a pandas.core.frame.DataFrame).

What kind of things does births_df contain? DataFrames have an attribute
called dtypes that answers this:

```python
births_df.dtypes
```

```output
year             int64
month            int64
date_of_month    int64
day_of_week      int64
births           int64
dtype: object
```

All the values in a single column have the same type. For example, values in the month
column have type int64, which is a kind of integer. Cells in the month column cannot have
fractional values, but values in weight and hindfoot\_length columns can, because they
have type float64. The object type doesn't have a very helpful name, but in
this case it represents strings (such as 'M' and 'F' in the case of sex).

We'll talk a bit more about what the different formats mean in a different lesson.

#### Useful Ways to View DataFrame Objects in Python

There are many ways to summarize and access the data stored in DataFrames,
using **attributes** and **methods** provided by the DataFrame object.

Attributes are features of an object. For example, the shape attribute will output
the size (the number of rows and columns) of an object. To access an attribute,
use the DataFrame object name followed by the attribute name df_object.attribute.
For example, using the DataFrame births_df and attribute columns, an index
of all the column names in the DataFrame can be accessed with births_df.columns.

Methods are like functions, but they only work on particular kinds of objects. As
an example, **the head() method** works on DataFrames. Methods are called in a
similar fashion to attributes, using the syntax df_object.method(). Using
births_df.head() gets the first few rows in the DataFrame births_df
using the head() method. With a method, we can supply extra information
in the parentheses to control behaviour.

Let's look at the data using these.




> ## Challenge - DataFrames
>
> Using our DataFrame births_df, try out the **attributes** & **methods** below to see
what they return.
> 1. births_df.columns
> 2. births_df.shape Take note of the output of shape - what format does it return the shape of the DataFrame in? HINT: [More on tuples, here](https://docs.python.org/3/tutorial/datastructures.html#tuples-and-sequences).
> 3. births_df.head() Also, what does births_df.head(15) do?
> 4. births_df.tail()
> 
>
> > ## Solution
> >
> > 1. births_df.columns provides the names of the columns in the DataFrame.
> > 2. births_df.shape provides the dimensions of the DataFrame as a tuple in (r,c) format, where r is the number of rows and c the number of columns.
> > 3. births_df.head() returns the first 5 lines of the DataFrame,  annotated with column and row labels. Adding an integer as an argument to the functionspecifies the number of lines to display from the top of the DataFrame, e.g. births_df.head(15) will return the first 15 lines.
> > 4. births_df.tail() will display the last 5 lines, 
   and behaves similarly to the head() method.
> > 
> {: .solution}
{: .challenge}



> ## Instructor Note: Recapping object (im)mutability
> Working through solutions to the challenge above
can provide a good opportunity to recap about mutability and immutability
of different objects.
> Show that the DataFrame index
( the columns attribute) is immutable, e.g. 
births_df.columns[4] = "dateofmonth" returns a TypeError.
> Adapting the name is done with the rename function:
> ```python
> births_df.rename(columns={"date_of_month": "dateofmonth"}))
> ```
> {: .source}
{: .callout}


### Calculating Statistics From Data In A Pandas DataFrame

We've read our data into Python. Next, let's perform some quick summary
statistics to learn more about the data that we're working with. We might want
to know how many animals were collected in each site, or how many of each
species were caught. We can perform summary stats quickly using groups. But
first we need to figure out what we want to group by.

Let's begin by exploring our data:

```python
# Look at the column names
births_df.columns
```

which **returns**:

```output
Index(['year', 'month', 'date_of_month', 'day_of_week', 'births'], dtype='object')

```

Let's get a list of all the years. The pd.unique function tells us all of
the unique values in the year column.

```python
pd.unique(births_df['year'])
```

which **returns**:

```output
array([2000, 2001, 2002, 2003, 2004, 2005, 2006, 2007, 2008, 2009, 2010,
       2011, 2012, 2013, 2014])
```



> ## Challenge - Statistics
> 
> 1. Create a list of unique month dates ("date\_of\_month") found in the surveys data. Call it month_dates. How many unique month dates are there in the data? How many unique weekdays are in the data?
> 2. What is the difference between len(month_dates) and births_df['date_of_month'].nunique()?
>
> > ## Solution
> > 1. month_dates = pd.unique(births_df["date_of_month"])
> > - How many unique sites are in the data?  
> > month_dates.size or len(month_dates) provide the answer: 31
> > - How many unique weekdays are in the data?
> > len(pd.unique(births_df["day_of_week"])) tells us there are 7
> > weekdays
> > 2. len(month_dates) and births_df['day_of_month'].nunique() 
> > both provide the same output: 
> > they are alternative ways of getting the unique values.
> > The nunique method combines the count and unique value extraction,
> > and can help avoid the creation of intermediate variables like 
> > month_dates.
> >
> {: .solution}
{: .challenge}


## Groups in Pandas

We often want to calculate summary statistics grouped by subsets or attributes
within fields of our data. For example, we might want to calculate the average
weight of all individuals per site.

We can calculate basic statistics for all records in a single column using the
syntax below:

```python
births_df['births'].describe()
```

gives **output**

```python
count     5479.000000
mean     11350.068261
std       2325.821049
min       5728.000000
25%       8740.000000
50%      12343.000000
75%      13082.000000
max      16081.000000
Name: births, dtype: float64
```

> ## Instructor: Important Bug Note
> In pandas prior to version 0.18.1 there is a bug causing births_df['weight'].describe() to return a runtime error.
> {: .source}
{: .callout}

We can also extract one specific metric if we wish:

```python
births_df['births'].min()
births_df['births'].max()
births_df['births'].mean()
births_df['births'].std()
births_df['births'].count()
```

But if we want to summarize by one or more variables, for example year, we can
use **Pandas' .groupby method**. Once we've created a groupby DataFrame, we
can quickly calculate summary statistics by a group of our choice.

```python
# Group data by year
grouped_data = births_df.groupby('year')
```

The **pandas function describe** will return descriptive stats including: mean,
median, max, min, std and count for a particular column in the data. Pandas'
describe function will only return summary values for columns containing
numeric data.

```python
# Summary statistics for all numeric columns by year
grouped_data.describe()
# Provide the mean for each numeric column by year
grouped_data.mean(numeric_only=True)
```

grouped_data.mean(numeric_only=True) **OUTPUT:**

![Group By Year](/assets/img/group-by-year.png)

The groupby command is powerful in that it allows us to quickly generate
summary stats.





> ## Challenge - Summary Data
> 
> 1. What happens when you group by two columns using the following syntax and then calculate mean values?
> - grouped_data2 = births_df.groupby(['year', 'month'])
> - grouped_data2.mean(numeric_only=True)
> 2. Summarize month values for each year in your data.
>
> > ## Solution
> > 
> > 1. Calling the mean() method on data grouped by these two columns  
> > calculates and returns the mean value for each combination of plot 
> > and sex. 
> > - Note that the mean is not meaningful for some variables,
> > e.g. day, month, and year. 
> > 2. births_df.groupby(['year'])['month'].describe()
> > ![Births By Year](/assets/img/births-by-year.png)
> > 
> {: .solution}
{: .challenge}


## Quickly Creating Summary Counts in Pandas

Let's next count the number of samples for each species. We can do this in a few
ways, but we'll use groupby combined with **a count() method**.

```python
# Count the number of births by year
year_counts = births_df.groupby('year')['births'].count()
print(year_counts)
```

Or, we can also count just the rows that have the species "DO":

```python
births_df.groupby('year')['births'].count()['1']
```


> ## Challenge - Make a list
> 
> What's another way to create a list of years and associated count of
> the birth dates in the data? Hint: you can perform count, min, etc.
> functions on groupby DataFrames in the same way you can perform them on
> regular DataFrames.
> > ## Solution
> > As well as calling count() on the births column of the grouped
> > DataFrame as above, an equivalent result can be obtained by 
> > extracting 
> > births from the result of count() called directly on the grouped DataFrame:
> {: .solution}
{: .challenge}



```python
births_df.groupby('year').count()['births']
```

```output
year
2000    366
2001    365
2002    365
2003    365
2004    366
2005    365
2006    365
2007    365
2008    366
2009    365
2010    365
2011    365
2012    366
2013    365
2014    365
Name: births, dtype: int64
```


### Basic Math Functions

If we wanted to, we could apply a mathematical operation like addition or division
on an entire column of our data. For example, let's multiply all weight values by 2.

```python
# Multiply all weight values by 2
births_df['births']*2
```

A more practical use of this might be to normalize the data according to a mean, area,
or some other value calculated from our data.

## Quick \& Easy Plotting Data Using Pandas

We can plot our summary stats using Pandas, too.

```python
# Make sure figures appear inline in Ipython Notebook
%matplotlib inline
# Create a quick bar chart
year_counts.plot(kind='bar');
```
![Year Chart](/assets/img/year-chart.png)

![](fig/countPerSpecies.png){alt='Weight by Species Site'}
Count per species site

We can also look at how many unique birth numbers were recorded

```python
total_count = births_df.groupby('year')['births'].nunique()
# Let's plot that too
total_count.plot(kind='bar');
```
![Unique Year Chart](/assets/img/year-chart-unique.png)

> ## Challenge - Plots
>
> Create a plot of total births per day in February 2012.
> > ## Solution
> > 
> > ```python
> > feb_2012 = births_df[(births_df['year'] == 2012) & (births_df['month'] == 2)]
> > births_per_day = feb_2012.groupby('date_of_month')['births'].sum()
> > births_per_day.plot(kind='bar');
> > ```
> >
> > ![Feb 2012 Births](/assets/img/feb-2012-births.png)
> > 
> > 
> {: .solution}
{: .challenge}




