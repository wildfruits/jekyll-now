---
title: KDD Challenge Part 2 - Feature Engineering 1 
date: 2017-07-10 17:35:55
tags:
---

Knowledge component (KC) as defined by PSLC is "a generalization of everyday terms like concept, principle, fact, or skill, and cognitive science terms like schema, production rule, misconception, or facet."

One way I can define proficiency at a KC is by observing the number of times a student answers a question containing a particular KC correctly on the first try as a fraction of how many times they encounter questions containing that KC. This is a simplified assessment; first, student performance tends to improve over time, so a more detailed model would take temporal relationships into account; second, students only see remedial problems if they don’t do well on the original problems they are given, so a more detailed model would take these causal relationships into account.

## Feature Extraction

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

## Metric

Because this data is imbalanced, (about 77% of the observations have target “Correct First Attempt” values of 1 and 23% have target values of 0), a naive model can achieve an accuracy of 77% by simply guessing 1 for every observation (the "most_frequent" strategy for a dummy classifier). This is not an acceptable solution because we also want to be able to identify negative cases accurately, i.e. students who do not answer questions correctly on the first try. This 77% “success” rate is due to the classes being imbalanced, not to the classifier being useful.

Some more useful metrics for this dataset are F-score and AUC/ROC, both of which provide the more detailed information I need about the model’s predictive power for both classes. In order to draw a meaningful baseline solution, I am using a dummy model with the default "stratified" strategy, which respects the original class frequency of the target variable.

F1 score (balanced F-score) is computed as follows:

![image](/images/f1 score.gif)

Precision, also called positive (or negative) predictive value in medical and other diagnostic contexts, can be articulated as “If the model says a value is 1 (or 0), what is the probability it is actually 1 (or 0)?” Recall, also called sensitivity, can be expressed as “If a value is 1 (or 0), what is the probability the model correctly identifies it as 1 (or 0)?” F1 score  is the harmonic mean of these two measures.

