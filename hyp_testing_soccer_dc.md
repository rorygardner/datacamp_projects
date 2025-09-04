![A soccer pitch for an international match.](soccer-pitch.jpg)

You're working as a sports journalist at a major online sports media company, specializing in soccer analysis and reporting. You've been watching both men's and women's international soccer matches for a number of years, and your gut instinct tells you that more goals are scored in women's international football matches than men's. This would make an interesting investigative article that your subscribers are bound to love, but you'll need to perform a valid statistical hypothesis test to be sure!

While scoping this project, you acknowledge that the sport has changed a lot over the years, and performances likely vary a lot depending on the tournament, so you decide to limit the data used in the analysis to only official `FIFA World Cup` matches (not including qualifiers) since `2002-01-01`.

You create two datasets containing the results of every official men's and women's international football match since the 19th century, which you scraped from a reliable online source. This data is stored in two CSV files: `women_results.csv` and `men_results.csv`.

The question you are trying to determine the answer to is:

> Are more goals scored in women's international soccer matches than men's?

You assume a **10% significance level**, and use the following null and alternative hypotheses:

$H_0$ : The mean number of goals scored in women's international soccer matches is the same as men's.

$H_A$ : The mean number of goals scored in women's international soccer matches is greater than men's.


```python
# Start your code here!
import pandas as pd
import seaborn as sns
import matplotlib.pyplot as plt
from scipy.stats import ttest_ind, mannwhitneyu, shapiro

# data
women_results = pd.read_csv("women_results.csv")
men_results = pd.read_csv("men_results.csv")

# filter: fifa
women_results_fifa = women_results.loc[women_results['tournament'] == "FIFA World Cup"]
men_results_fifa = men_results.loc[men_results['tournament'] == "FIFA World Cup"]

# filter: dates
women_results_fifa['date'] = pd.to_datetime(women_results_fifa['date'])
men_results_fifa['date'] = pd.to_datetime(men_results_fifa['date'])

women_results_filt = women_results_fifa.loc[women_results_fifa['date'] >= '2002-01-01']
men_results_filt = men_results_fifa.loc[men_results_fifa['date'] >= '2002-01-01']

# total_goals = home + away 
women_results_filt['total_goals'] = women_results_filt['home_score'] + women_results_filt['away_score']
men_results_filt['total_goals'] = men_results_filt['home_score'] + men_results_filt['away_score']
```


```python
# normality
sns.displot(women_results_filt['total_goals'], color = "red", kind = "kde")
sns.displot(men_results_filt['total_goals'], color = "blue", kind = "kde")

# shapiro wilk
print(shapiro(women_results_filt['total_goals']))
print(shapiro(men_results_filt['total_goals']))
```

    ShapiroResult(statistic=0.8491013050079346, pvalue=3.8905201759850683e-13)
    ShapiroResult(statistic=0.9266489744186401, pvalue=8.894154401688226e-13)



    
![png](hyp_testing_soccer_dc_files/hyp_testing_soccer_dc_2_1.png)
    



    
![png](hyp_testing_soccer_dc_files/hyp_testing_soccer_dc_2_2.png)
    



```python
# mannwhitneyu?
u_stat, p_val_u = mannwhitneyu(
    women_results_filt['total_goals'], 
    men_results_filt['total_goals'], 
    alternative="greater")
print(p_val_u)

result_dict = {"p_val": p_val_u, "result": "reject"}
```

    0.005106609825443641

