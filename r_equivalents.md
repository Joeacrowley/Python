---
title: R Equivalents
format:
  html:
    toc: true
    toc-depth: 4
    toc-location: right
---

## Python Syntax Basics

Learning the syntax style and conventions is an important part of the basics. Python's rules are simpler than R's in some ways, but they are also stricter about formatting. The main things worth learning early are below.

------------------------------------------------------------------------

## 1. Function Calls and Arguments

Python is very similar to R here.

Arguments go inside parentheses and are comma-separated.

``` python
func(a, b, c)
```

Keyword arguments are also similar:

``` python
func(x=1, y=2)
```

You can mix positional and named arguments (positional must come first):

``` python
func(1, y=2)
```

------------------------------------------------------------------------

## 2. Spreading Arguments Over Multiple Lines

Like R, Python allows multi-line argument lists.

``` python
result = some_function(
    argument1,
    argument2,
    option=True,
    size=10
)
```

This is common and considered good style when lines get long.

Trailing commas are allowed and often used:

``` python
some_function(
    a,
    b,
    c,
)
```

------------------------------------------------------------------------

## 3. Indentation Defines Structure

This is the biggest syntax difference from R.

Python does not use braces `{}`.\
Blocks are defined by indentation.

``` python
if x > 0:
    print("positive")
    y = x + 1
```

The colon `:` starts a block, and everything indented underneath belongs
to it.

Typical indentation is **4 spaces**.

------------------------------------------------------------------------

## 4. Line Break Rules

Statements normally end at a newline:

``` python
x = 5
y = x + 2
```

Python allows implicit continuation inside:

-   parentheses `()`
-   brackets `[]`
-   braces `{}`

Example:

``` python
total = (
    a
    + b
    + c
)
```

Without those, you need a backslash:

``` python
total = a + b + \
        c
```

This style is usually avoided.

------------------------------------------------------------------------

## 5. Assignment

Python uses `=` for assignment.

``` python
x = 10
```

Unlike R, there is no `<-`.

Multiple assignment is common:

``` python
a, b = 1, 2
```

This is often used for swapping:

``` python
a, b = b, a
```

------------------------------------------------------------------------

## 6. Comments

Single line comments use `#`.

``` python
# this is a comment
```

There is no dedicated multi-line comment syntax; people usually write
several `#` lines.

Docstrings are used for documentation:

``` python
def func(x):
    """Short description of function."""
    return x * 2
```

Docstrings are just strings placed inside code to describe what something does. They’re mainly used for functions, classes, and modules. That triple-quoted string is the docstring. It sits right under the definition line.

They’re not comments; Python actually keeps them around at runtime.

You can access a docstring directly:

```
add.__doc__
```

You might write:

```
def divide(a, b):
    """
    Divide a by b.

    Parameters:
        a (float): numerator
        b (float): denominator

    Returns:
        float: result of division
    """
```

------------------------------------------------------------------------

## 7. Indexing

Indexing uses square brackets and starts at **0**, unlike R.

``` python
x[0]
```

Slices use a colon:

``` python
x[0:3]
```

Meaning: elements `0, 1, 2`.

------------------------------------------------------------------------

## 8. Method Style

Python frequently attaches functions to objects.

``` python
text.lower()
df.head()
arr.mean()
```

In R you more often see:

``` r
tolower(text)
head(df)
mean(arr)
```

------------------------------------------------------------------------

## 9. Naming Conventions

Python has widely followed conventions:

  Item        Convention
  ----------- --------------
  variables   `snake_case`
  functions   `snake_case`
  classes     `PascalCase`
  constants   `UPPER_CASE`

Example:

``` python
total_count = 10

class LinearModel:
    pass
```

These are described in **PEP 8**, the Python style guide.

------------------------------------------------------------------------

## 10. Imports

Imports normally go at the top of a file.

``` python
import pandas as pd
import numpy as np
```

Aliases are extremely common.

------------------------------------------------------------------------

## A Simple R vs Python Example

**R**

``` r
result <- mean(
  x,
  na.rm = TRUE
)
```

**Python**

``` python
result = np.mean(
    x,
    where=~np.isnan(x)
)
```

Structurally very similar --- parentheses, commas, and optional line breaks.

------------------------------------------------------------------------

## Core Syntax Things Worth Memorising

If you internalise just these, Python code becomes easy to read:

-   indentation defines blocks
-   `:` starts a block
-   `=` assigns
-   `()` call functions
-   `[]` index or slice
-   `.` accesses methods or attributes
-   `import` loads modules


| R | Python |
|---|---|
| `<-` | `=` |
| `=` (in functions) | `=` (keyword args) |
| `c(1,2,3)` | `[1, 2, 3]` |
| `list()` | `dict()` |
| `vector()` | `list()` |
| `matrix()` | `np.array()` |
| `array()` | `np.ndarray()` |
| `data.frame()` | `pd.DataFrame()` |
| `tibble()` | `pd.DataFrame()` |
| `factor()` | `pd.Categorical()` |
| `NULL` | `None` |
| `NA` | `np.nan` |
| `TRUE` / `FALSE` | `True` / `False` |
| `nrow()` / `ncol()` | `df.shape[0]` / `df.shape[1]` |
| `length()` | `len()` |
| `str()` | `df.info()` |
| `summary()` | `df.describe()` |
| `head()` / `tail()` | `df.head()` / `df.tail()` |
| `print()` | `print()` |
| `paste()` / `paste0()` | `f""` f-strings |
| `sprintf()` | `f""` f-strings |
| `is.na()` | `pd.isna()` / `.isnull()` |
| `na.omit()` | `df.dropna()` |
| `which()` | `np.where()` |
| `seq(1, 10)` | `range(1, 11)` |
| `seq(1, 10, by=2)` | `range(1, 11, 2)` |
| `rep()` | `[x] * n` |
| `ifelse()` | `np.where()` |
| `for (i in x)` | `for i in x:` |
| `function(x) {}` | `def f(x):` |
| `apply()` | `df.apply()` |
| `lapply()` | `map()` / list comprehension |
| `sapply()` | `[f(x) for x in list]` |
| `library(dplyr)` | `import pandas as pd` |
| `library(ggplot2)` | `import matplotlib.pyplot as plt` / `import seaborn as sns` |
| `library(tidyr)` | `import pandas as pd` |
| `library(stringr)` | `import re` / `str` methods |
| `library(lubridate)` | `import datetime` / `pd.to_datetime()` |
| `select()` | `df[['col1', 'col2']]` |
| `filter()` | `df[df['col'] > x]` |
| `mutate()` | `df['new_col'] = ...` |
| `arrange()` | `df.sort_values()` |
| `group_by() %>% summarise()` | `df.groupby().agg()` |
| `pivot_wider()` | `df.pivot()` |
| `pivot_longer()` | `df.melt()` |
| `left_join()` | `pd.merge(..., how='left')` |
| `%>%` pipe | `.` method chaining |
| `lm()` | `sklearn.linear_model.LinearRegression()` |
| `glm()` | `sklearn.linear_model.LogisticRegression()` |
| `cor()` | `df.corr()` |
| `t.test()` | `scipy.stats.ttest_ind()` |
| `set.seed()` | `np.random.seed()` |
| RMarkdown | Jupyter notebook |
| `.Rproj` | `pyproject.toml` |
| `renv` | conda / venv |
| CRAN | PyPI |
| `?function` | `help(function)` / `function?` in Jupyter |
| `Ctrl+Enter` (run line) | `Shift+Enter` (run cell) |