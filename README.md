# MadisonSpendingData
Project to analyze the spending data for 5 public agencies in Madison, WI from 2015 to 2018


## Data

Data from `madison.csv` with spending for the 5 agencies from 2015 to 2018


agency_id|agency|2015|2016|2017|2018
------|------|------|------|------|------
5|police|68.06346877|71.32575615000002|73.24794765999998|77.87553504
6|fire|49.73757877|51.96834048|53.14405332|55.215007260000014
9|library|16.96543425|18.12552139|19.13634773|19.845065799999997
12|parks|18.371421039999998|19.159243279999995|19.316837019999994|19.7607100000000
15|streets|25.368879940000006|28.2286218|26.655754419999994|27.798933740000003

## CSV Analysis Functions

Using the get_id function from `project.py` it allows us to get the id number of the agency from the csv file:

```python
def get_id(agency):
    """get_id(agency) returns the ID of the specified agency."""
    if __agency_to_id == None:
        raise Exception("you did not call init first")
    if not agency in __agency_to_id:
        raise Exception("No agency '%s', only these: %s" %
                        (str(agency), ','.join(list(__agency_to_id.keys()))))
    return __agency_to_id[agency]
```

Then using the get_spending function from `project.py` we can get the spending of an agency from a specific year for better data analysis:

```python
def get_spending(agency_id, year=2018):
    """get_spending(agency_id, year) returns the dollars spent (in millions) by the specified agency in specified year."""
    if __data == None:
        raise Exception("you did not call init first")
    if not (agency_id, year) in __data:
        raise Exception("No data for agency %s, in year %s" %
                        (str(agency_id), str(year)))
    return __data[(agency_id, year)]
```

## Spending Analysis

Now that we have the functions from `project.py` we can access the data in `madison.csv`

First I tested my functions, setting the IDs of `Streets` and `Library` to variables and getting the spending for `streets_id` in 2017:

<img src="img/Get Spending For Streets.png" width="500">

### Maximum Spending

Next I created a new function to find the maximum spent by one agency in a given year:

```python
def year_max(year):
    # grab the spending by each agency in the given year
    police_spending = project.get_spending(project.get_id("police"), year)
    fire_spending = project.get_spending(project.get_id("fire"), year)
    library_spending = project.get_spending(project.get_id("library"), year)
    parks_spending = project.get_spending(project.get_id("parks"), year)
    streets_spending = project.get_spending(project.get_id("streets"), year)

    # use builtin max function to get the largest of the five values
    return max(police_spending, fire_spending, library_spending, parks_spending, streets_spending)
```

Given this function I was able to find the maximum spending in `2015` and `2018`:

<img src="img/Year Max Results.png" width="500">

### Agency Minimum Spending

I then created a function to return the least money spent by an agency in all of the available years of spending:

```python
#to find minimum spending by an agency
#in years available in data
def agency_min(agency):
    
    agency_id = project.get_id(agency)
    y15 = project.get_spending(agency_id, 2015)
    y16 = project.get_spending(agency_id, 2016)
    y17 = project.get_spending(agency_id, 2017)
    y18 = project.get_spending(agency_id, 2018)

    return min(y15, y16, y17, y18)
```
Returning me the results for the minimum spent by `police`, `library`, and `parks` in 2015 to 2018:

<img src="img/Agency Min Results.png" width="400">

### Agency Average Spending

Next I wanted to find the average spent by an agency each year:

```python
#Average spending by agency in data available
def agency_avg(agency):
    
    agency_id = project.get_id(agency)
    y15 = project.get_spending(agency_id, 2015)
    y16 = project.get_spending(agency_id, 2016)
    y17 = project.get_spending(agency_id, 2017)
    y18 = project.get_spending(agency_id, 2018)

    return (y15 + y16 + y17 + y18)/4
```

This allowed me to find the average spending for `streets` and `fire`agencies as well as the percentage spent above average by `police` in 2018:

<img src="img/Agency Avg Results.png" width="400">

### Average Change Per Year

Given the input of an agency and timeline , the `change_per_year` function returns the average change each year of an agency in these years:

```python
def change_per_year(agency, start_year=2015, end_year=2018):
    agency_id = project.get_id(agency)
    yr1_tot = project.get_spending(agency_id, start_year)
    yr4_tot = project.get_spending(agency_id, end_year)
    return (yr4_tot - yr1_tot)/(end_year - start_year)
```

This gives me the average change for `police` starting in 2015 as well as starting in 2017 and `streets` starting in 2016:

<img src="img/Average Change Results.png" width="500">

### Agency Extrapolation

I then wanted to find the potential spending for agencies in the future, so I used the average change in spending data I had to predict the spending in future years using the `extrapolate` function:

```python
def extrapolate(agency, y1, y2, y3):
    agency_id = project.get_id(agency)
    y1_tot = project.get_spending(agency_id, y1)
    y2_tot = project.get_spending(agency_id, y2)
    avg_change = (y2_tot - y1_tot)/(y2-y1)
    change_yr = y3 - y2
    return y2_tot + (avg_change * change_yr)
```

This yielded the results for future spending of the `library` in `2019` and `2100`:

<img src="img/Extrapolation Results.png" width="500">

### Extrapolation Error

I then created a function to find the error in my extrapolation function using known data points in `madison.csv`

```python
def extrapolate_error(agency, y1, y2, ext_yr):
    agency_id = project.get_id(agency)
    est = extrapolate(agency = agency, y1 = y1, y2 =y2, y3 = ext_yr)
    actual = project.get_spending(agency_id, ext_yr)
    return est - actual
```

This shows the errors for `police` and `streets` with the `extrapolation` function:

<img src="img/Extrapolation Error Results.png" width="500">

### Standard Deviation

Finally I wanted to find the standard deviation of spending for `library` with my given data using the function:

```python
lib_15 = project.get_spending(library_id, 2015)
lib_16 = project.get_spending(library_id, 2016)
lib_17 = project.get_spending(library_id, 2017)
lib_18 = project.get_spending(library_id, 2018)

lib_avg = (lib_15 + lib_16 + lib_17 + lib_18)/4
dev_15 = (lib_15 - lib_avg) ** 2
dev_16 = (lib_16 - lib_avg) ** 2
dev_17 = (lib_17 - lib_avg) ** 2
dev_18 = (lib_18 - lib_avg) ** 2

dev_avg = (dev_15 + dev_16 + dev_17 + dev_18)/4

dev_avg ** (1/2)
```
yielding the result of `1.0848913984858986`
