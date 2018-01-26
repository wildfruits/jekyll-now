---
title: KDD Challenge Part 2 - Feature Engineering 1 - Adding Feature Columns
date: 2017-07-10 17:35:55
tags:
---

Recall that we want to be able to sort problems into categories with descriptive labels. We would like to know if a problem is an algebra problem, geometry problem, etc. and break it down further from there. We will be using feature engineering to create additional feature columns for this dataset that we can use to discover relationships within our data.

## Additional Features

One way to extract multiple columns from a single column using strings with a known character delimiter is **str.split()**. Because **KC(Default)** and **Opportunity(Default)** both use **~~** as a delimiter, we can split on this substring.

```python
df[['KC1', 'KC2', 'KC3', 'KC4', 'KC5', 'KC6', 'KC7']] = df['KC(Default)'].str.split('~~', expand=True)
df[['Opps1', 'Opps2', 'Opps3', 'Opps4', 'Opps5', 'Opps6', 'Opps7']] = df['Opportunity(Default)'].str.split('~~', expand=True)
```

This splits the **KC(Default)** and **Opportunity(Default)** columns at every point where we encounter a **~~** delimiter that indicates a separate KC or corresponding Opportunity count.

We convert the Opportunity column data types from string to numeric and drop the time-related columns since we won't be using them for this part of our analysis.

```python
df['Opps1'] = pd.to_numeric(df['Opps1'])
...
df = df.drop('Step Start Time', 1)
df = df.drop('Step Start Time', 1)
df = df.drop('Step End Time', 1)
...
df = df.drop('Correct Step Duration (sec)', 1)
...
```
This gives us a dataframe with multiple KC and Opportunity columns corresponding to each KC a student encounters in a student-step record. Our goal at this stage is to get a list of each KC each student encounters and the total number of times they encounter that KC, given by the highest Opportunity count corresponding to that KC. We will then determine the number of Correct First Attempts that student achieved for that KC and get a ratio of CFAs to total Opportunity count. This will be discussed in more detail in the next post.

**Next time:** We'll use groupby to get counts of the KCs each student encounters and use this to calculate the ratio of Correct First Attempts to total Opportunity count per KC.
