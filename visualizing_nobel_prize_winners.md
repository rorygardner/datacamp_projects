The Nobel Prize has been among the most prestigious international awards since 1901. Each year, awards are bestowed in chemistry, literature, physics, physiology or medicine, economics, and peace. In addition to the honor, prestige, and substantial prize money, the recipient also gets a gold medal with an image of Alfred Nobel (1833 - 1896), who established the prize.

![](Nobel_Prize.png)

The Nobel Foundation has made a dataset available of all prize winners from the outset of the awards from 1901 to 2023. The dataset used in this project is from the Nobel Prize API and is available in the `nobel.csv` file in the `data` folder.

In this project, you'll get a chance to explore and answer several questions related to this prizewinning data. And we encourage you then to explore further questions that you're interested in!


```python
# Loading in required libraries
import pandas as pd
import seaborn as sns
import numpy as np

# Start coding here!

#data
nobel = pd.read_csv('data/nobel.csv')
nobel.columns
nobel.head()
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>year</th>
      <th>category</th>
      <th>prize</th>
      <th>motivation</th>
      <th>prize_share</th>
      <th>laureate_id</th>
      <th>laureate_type</th>
      <th>full_name</th>
      <th>birth_date</th>
      <th>birth_city</th>
      <th>birth_country</th>
      <th>sex</th>
      <th>organization_name</th>
      <th>organization_city</th>
      <th>organization_country</th>
      <th>death_date</th>
      <th>death_city</th>
      <th>death_country</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>1901</td>
      <td>Chemistry</td>
      <td>The Nobel Prize in Chemistry 1901</td>
      <td>"in recognition of the extraordinary services ...</td>
      <td>1/1</td>
      <td>160</td>
      <td>Individual</td>
      <td>Jacobus Henricus van 't Hoff</td>
      <td>1852-08-30</td>
      <td>Rotterdam</td>
      <td>Netherlands</td>
      <td>Male</td>
      <td>Berlin University</td>
      <td>Berlin</td>
      <td>Germany</td>
      <td>1911-03-01</td>
      <td>Berlin</td>
      <td>Germany</td>
    </tr>
    <tr>
      <th>1</th>
      <td>1901</td>
      <td>Literature</td>
      <td>The Nobel Prize in Literature 1901</td>
      <td>"in special recognition of his poetic composit...</td>
      <td>1/1</td>
      <td>569</td>
      <td>Individual</td>
      <td>Sully Prudhomme</td>
      <td>1839-03-16</td>
      <td>Paris</td>
      <td>France</td>
      <td>Male</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>1907-09-07</td>
      <td>Châtenay</td>
      <td>France</td>
    </tr>
    <tr>
      <th>2</th>
      <td>1901</td>
      <td>Medicine</td>
      <td>The Nobel Prize in Physiology or Medicine 1901</td>
      <td>"for his work on serum therapy, especially its...</td>
      <td>1/1</td>
      <td>293</td>
      <td>Individual</td>
      <td>Emil Adolf von Behring</td>
      <td>1854-03-15</td>
      <td>Hansdorf (Lawice)</td>
      <td>Prussia (Poland)</td>
      <td>Male</td>
      <td>Marburg University</td>
      <td>Marburg</td>
      <td>Germany</td>
      <td>1917-03-31</td>
      <td>Marburg</td>
      <td>Germany</td>
    </tr>
    <tr>
      <th>3</th>
      <td>1901</td>
      <td>Peace</td>
      <td>The Nobel Peace Prize 1901</td>
      <td>NaN</td>
      <td>1/2</td>
      <td>462</td>
      <td>Individual</td>
      <td>Jean Henry Dunant</td>
      <td>1828-05-08</td>
      <td>Geneva</td>
      <td>Switzerland</td>
      <td>Male</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>1910-10-30</td>
      <td>Heiden</td>
      <td>Switzerland</td>
    </tr>
    <tr>
      <th>4</th>
      <td>1901</td>
      <td>Peace</td>
      <td>The Nobel Peace Prize 1901</td>
      <td>NaN</td>
      <td>1/2</td>
      <td>463</td>
      <td>Individual</td>
      <td>Frédéric Passy</td>
      <td>1822-05-20</td>
      <td>Paris</td>
      <td>France</td>
      <td>Male</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>1912-06-12</td>
      <td>Paris</td>
      <td>France</td>
    </tr>
  </tbody>
</table>
</div>




```python
## 1. top gender and birth country
# gender
top_gender = nobel["sex"].value_counts().index[0]
print(top_gender)

# country
top_country = nobel["birth_country"].value_counts().index[0]
print(top_country)
```

    Male
    United States of America



```python
## 2. decade
nobel["year"].max()
nobel["year"].min()

# decades column
nobel["decade"] = (nobel["year"]//10)*10

# pivot table by decade and country
pivot_dec = pd.pivot_table(nobel, index = "decade", columns = "birth_country", 
                           aggfunc = "size", fill_value = 0)

# total winners by decade
pivot_dec["dec_total"] = pivot_dec.sum(axis = 1)

# usa/total
pivot_dec["usa_to_total"] = round(pivot_dec["United States of America"]/pivot_dec["dec_total"], 3)

# select max usa/total
max_decade_usa = pivot_dec["usa_to_total"].idxmax()
print(max_decade_usa)
```

    2000



```python
## 3. decade and category with highest proportion of female as dictionary

# gender as boolean
nobel["female"] = (nobel["sex"] == "Female").astype(bool)

# group by decade and category, mean for female t/f
proportions_nobel = nobel.groupby(["decade", "category"])["female"].mean().reset_index()

# max female
max_fem_idx = proportions_nobel["female"].idxmax()

max_female_dict = {
    proportions_nobel.loc[max_fem_idx, "decade"]:proportions_nobel.loc[max_fem_idx, "category"]}
print(max_female_dict)

```

    {2020: 'Literature'}



```python
## 4. first woman to receive nobel prize, what category?

# females df
females_nobel = nobel[nobel["sex"] == "Female"]

# first
first_woman = females_nobel[
    females_nobel["year"] == females_nobel["year"].min()].reset_index()

# store
first_woman_name = first_woman.loc[0, "full_name"]
first_woman_category = first_woman.loc[0, "category"]
print(first_woman_name)
print(first_woman_category)
```

    Marie Curie, née Sklodowska
    Physics



```python
## individuals or organizations that have won >1 nobel prize? 
name_counts = nobel["full_name"].value_counts()

# >1
repeat_list = name_counts[name_counts>1].index.tolist()
repeat_list
```




    ['Comité international de la Croix Rouge (International Committee of the Red Cross)',
     'Linus Carl Pauling',
     'John Bardeen',
     'Frederick Sanger',
     'Marie Curie, née Sklodowska',
     'Office of the United Nations High Commissioner for Refugees (UNHCR)']


