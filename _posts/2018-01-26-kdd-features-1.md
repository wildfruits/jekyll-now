---
title: KDD Challenge Part 2 - Feature Engineering 1 
date: 2017-07-10 17:35:55
tags:
---

Knowledge component (KC) as defined by PSLC is "a generalization of everyday terms like concept, principle, fact, or skill, and cognitive science terms like schema, production rule, misconception, or facet."

One way I can define proficiency at a KC is by observing the number of times a student answers a question containing a particular KC correctly on the first try as a fraction of how many times they encounter questions containing that KC. This is a simplified assessment; first, student performance tends to improve over time, so a more detailed model would take temporal relationships into account; second, students only see remedial problems if they don’t do well on the original problems they are given, so a more detailed model would take these causal relationships into account.

## Additional Features

One way to extract multiple columns from a single column using strings with a known character delimiter is **str.split()**. Because **KC(Default)** and **Opportunity(Default)** both use **~~** as a delimiter, we can split on this substring.

```python
df[['KC1', 'KC2', 'KC3', 'KC4', 'KC5', 'KC6', 'KC7']] = df['KC(Default)'].str.split('~~', expand=True)
df[['Opps1', 'Opps2', 'Opps3', 'Opps4', 'Opps5', 'Opps6', 'Opps7']] = df['Opportunity(Default)'].str.split('~~', expand=True)
```

This splits the **KC(Default)** and **Opportunity(Default)** columns at every point where we encounter a **~~** delimiter that indicates a separate KC or corresponding Opportunity count.

I convert the Opportunity column data types from string to numeric and drop the time-related columns since I won't be using them for this part of the analysis.

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
This gives us a dataframe with multiple KC and Opportunity columns corresponding to each KC a student encounters in a student-step record. 
