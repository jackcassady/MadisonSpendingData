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

<img src="img/Get Spending For Streets.png" width="400">


