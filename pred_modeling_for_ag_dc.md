# Sowing Success: How Machine Learning Helps Farmers Select the Best Crops

![Farmer in a field](farmer_in_a_field.jpg)

Measuring essential soil metrics such as nitrogen, phosphorous, potassium levels, and pH value is an important aspect of assessing soil condition. However, it can be an expensive and time-consuming process, which can cause farmers to prioritize which metrics to measure based on their budget constraints.

Farmers have various options when it comes to deciding which crop to plant each season. Their primary objective is to maximize the yield of their crops, taking into account different factors. One crucial factor that affects crop growth is the condition of the soil in the field, which can be assessed by measuring basic elements such as nitrogen and potassium levels. Each crop has an ideal soil condition that ensures optimal growth and maximum yield.

A farmer reached out to you as a machine learning expert for assistance in selecting the best crop for his field. They've provided you with a dataset called `soil_measures.csv`, which contains:

- `"N"`: Nitrogen content ratio in the soil
- `"P"`: Phosphorous content ratio in the soil
- `"K"`: Potassium content ratio in the soil
- `"pH"` value of the soil
- `"crop"`: categorical values that contain various crops (target variable).

Each row in this dataset represents various measures of the soil in a particular field. Based on these measurements, the crop specified in the `"crop"` column is the optimal choice for that field.  

In this project, you will build multi-class classification models to predict the type of `"crop"` and identify the single most importance feature for predictive performance.


```python
# All required libraries are imported here for you.
import pandas as pd
from sklearn.linear_model import LogisticRegression
from sklearn.model_selection import train_test_split
from sklearn import metrics

# Load the dataset
crops = pd.read_csv("soil_measures.csv")
crops.isna().sum()

# factorize target
crops_nums, crops_labs = pd.factorize(crops['crop'])
crops['crops_nums'] = crops_nums
crops.drop("crop", axis = 1, inplace=True)
crops.head()

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
      <th>N</th>
      <th>P</th>
      <th>K</th>
      <th>ph</th>
      <th>crops_nums</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>90</td>
      <td>42</td>
      <td>43</td>
      <td>6.502985</td>
      <td>0</td>
    </tr>
    <tr>
      <th>1</th>
      <td>85</td>
      <td>58</td>
      <td>41</td>
      <td>7.038096</td>
      <td>0</td>
    </tr>
    <tr>
      <th>2</th>
      <td>60</td>
      <td>55</td>
      <td>44</td>
      <td>7.840207</td>
      <td>0</td>
    </tr>
    <tr>
      <th>3</th>
      <td>74</td>
      <td>35</td>
      <td>40</td>
      <td>6.980401</td>
      <td>0</td>
    </tr>
    <tr>
      <th>4</th>
      <td>78</td>
      <td>42</td>
      <td>42</td>
      <td>7.628473</td>
      <td>0</td>
    </tr>
  </tbody>
</table>
</div>




```python
# make models
X = crops.drop("crops_nums", axis=1).values
y = crops[['crops_nums']].values
X_train, X_test, y_train, y_test = train_test_split(X, y, test_size = 0.2, random_state = 2)

indeces = [0,1,2,3]
feat_scores = {}
for i in indeces:
    X_train_sing = X_train[:,i].reshape(-1,1)
    X_test_sing = X_test[:,i].reshape(-1,1)

    logreg = LogisticRegression(multi_class='multinomial', solver='lbfgs')
    logreg.fit(X_train_sing, y_train)

    y_pred = logreg.predict(X_test_sing)
    accuracy = metrics.accuracy_score(y_test, y_pred)

    feat_scores[i] = accuracy

print(feat_scores)
print(feat_scores[2])
best_predictive_feature = {"K": feat_scores[2]}
```

    {0: 0.12727272727272726, 1: 0.18409090909090908, 2: 0.3340909090909091, 3: 0.10681818181818181}
    0.3340909090909091

