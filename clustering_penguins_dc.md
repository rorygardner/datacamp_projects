![Alt text](https://imgur.com/orZWHly.png=80)
source: @allison_horst https://github.com/allisonhorst/penguins

You have been asked to support a team of researchers who have been collecting data about penguins in Antartica! The data is available in csv-Format as `penguins.csv`

**Origin of this data** : Data were collected and made available by Dr. Kristen Gorman and the Palmer Station, Antarctica LTER, a member of the Long Term Ecological Research Network.

**The dataset consists of 5 columns.**

Column | Description
--- | ---
culmen_length_mm | culmen length (mm)
culmen_depth_mm | culmen depth (mm)
flipper_length_mm | flipper length (mm)
body_mass_g | body mass (g)
sex | penguin sex

Unfortunately, they have not been able to record the species of penguin, but they know that there are **at least three** species that are native to the region: **Adelie**, **Chinstrap**, and **Gentoo**.  Your task is to apply your data science skills to help them identify groups in the dataset!


```python
# Import Required Packages
import pandas as pd
import matplotlib.pyplot as plt
from sklearn.cluster import KMeans
from sklearn.preprocessing import StandardScaler

# Loading and examining the dataset
penguins_df = pd.read_csv("penguins.csv")
penguins_df.head()

# encode sex 
penguins_df['sex'] = penguins_df['sex'].map({'MALE':0, 'FEMALE':1})

# preprocessing
scaler = StandardScaler()
scaled_penguins = scaler.fit_transform(penguins_df)

# kmeans
model = KMeans(n_clusters=3)
model.fit(scaled_penguins)
clusters = model.predict(scaled_penguins)

# clusters + og
penguins_df['cluster'] = clusters

# make df
penguins_df_num = penguins_df.drop('sex', axis=1)
stat_penguins = penguins_df.groupby('cluster').mean()

stat_penguins.head()
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
      <th>culmen_length_mm</th>
      <th>culmen_depth_mm</th>
      <th>flipper_length_mm</th>
      <th>body_mass_g</th>
      <th>sex</th>
    </tr>
    <tr>
      <th>cluster</th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>43.878302</td>
      <td>19.111321</td>
      <td>194.764151</td>
      <td>4006.603774</td>
      <td>0.000000</td>
    </tr>
    <tr>
      <th>1</th>
      <td>40.217757</td>
      <td>17.611215</td>
      <td>189.046729</td>
      <td>3419.158879</td>
      <td>1.000000</td>
    </tr>
    <tr>
      <th>2</th>
      <td>47.568067</td>
      <td>14.996639</td>
      <td>217.235294</td>
      <td>5092.436975</td>
      <td>0.487395</td>
    </tr>
  </tbody>
</table>
</div>


