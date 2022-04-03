---
layout: post
title:  "Turning a Python dictionary into a pandas DataFrame"
date:   2022-04-02 17:21:10 -0500
categories: 
---
One hard part of learning things online is that teaching materials, and students (including me!), are often **imperfect.** In a [DataCamp](https://www.datacamp.com/) exercise, I misread some instructions about turning a Python dictionary into a DataFrame, and I went down a rabbit hole as a result. This is what I learned.

## Dictionaries to DataFrames

### The easy way

The structure of your dictionary determines how hard or easy it is to make it into a pandas DataFrame. If your dictionary resembles a DataFrame, by having **keys that correspond to column labels** and values that correspond to a list of values in that column, the conversion is easy. Say you have this:
```python
my_dict = {"years": [1999, 2000, 2001], 
           "num_friends": [5, 3, 0]}
``` 
Making this into a DataFrame is trivial (assume pandas is already imported as `pd`):
```python
my_df = pd.DataFrame(my_dict) # no method needed
```
The resulting DataFrame `my_df` looks like this:
```python
   years  num_friends
0   1999            5
1   2000            3
2   2001            0
```
### The hard way
If your dictionary does *not* look like a DataFrame, it's trickier to get it into this kind of a shape. Suppose your dictionary has basically the same information as the previous one, but it just maps years to numbers of friends, like so:

```python
my_dict = {"1999": 5, "2000": 3, "2001":  0}
```
If you dump this into a DataFrame—which requires passing it as a list, otherwise you get an error about using all scalar values [\(see explanation at Statology](https://www.statology.org/valueerror-if-using-all-scalar-values-you-must-pass-an-index/)\)—you get the **keys as columns**. So from this:
```python
my_df = pd.DataFrame([my_dict]) # note brackets
```
`my_df` now looks like this:
```python
   1999  2000  2001
0     5     3     0
```
If you were trying to get the years and friend numbers in their own columns, you might try to use the [`DataFrame.from_dict()` method](https://pandas.pydata.org/pandas-docs/stable/reference/api/pandas.DataFrame.from_dict.html). And you *can* get columnwise orientation by doing this:
```python
my_df_cols = pd.DataFrame.from_dict(
    my_dict, 
    orient = "index", # this flips the orientation
    columns = ["num_friends"] # this name is arbitrary
)
```
The result, `my_df_cols`, looks like this:
```python
      num_friends
1999            5
2000            3
2001            0
```
This is better, but we want the years in their own column, not serving as row names. To do this, instead of passing in `my_dict`, you pass in its **`items()`**, like so ([approach inferred from StackOverflow](https://stackoverflow.com/questions/65427126/pandas-convert-dictionary-to-dataframe-where-keys-and-values-are-the-columns)), *without* using `from_dict()`:
```python
my_df_cols = pd.DataFrame(
    my_dict.items(), # pass in keys, values as items
    columns = ["year", "num_friends"]
)
```
The resulting DataFrame `my_df_cols` looks exactly like what we got from the first dictionary, in which the keys were column labels:
```python
   year  num_friends
0  1999            5
1  2000            3
2  2001            0
```
For now, that's what I know! 