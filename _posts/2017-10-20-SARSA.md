---
layout: post
title: SARSA
---

The 2005-2006 algebra training set, for context, contains 112 unique knowledge component strings. Transactions containing a single knowledge component make up just over 50% of the dataset (about 400,000 observations). Because 80% of these transactions have positive target values (Correct First Attempt = 1) a fully naïve model that labeled every target value as positive would be guaranteed to misclassify 20% of these transactions, or 10% of the dataset.
