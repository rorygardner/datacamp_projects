<center><img src="car.jpg" width=500></center>


Insurance companies invest a lot of time and money into optimizing their pricing and accurately estimating the likelihood that customers will make a claim. In many countries insurance it is a legal requirement to have car insurance in order to drive a vehicle on public roads, so the market is very large!

(`Source: https://www.accenture.com/_acnmedia/pdf-84/accenture-machine-leaning-insurance.pdf`) 

Knowing all of this, On the Road car insurance have requested your services in building a model to predict whether a customer will make a claim on their insurance during the policy period. As they have very little expertise and infrastructure for deploying and monitoring machine learning models, they've asked you to identify the single feature that results in the best performing model, as measured by accuracy, so they can start with a simple model in production.

They have supplied you with their customer data as a csv file called `car_insurance.csv`, along with a table detailing the column names and descriptions below.



## The dataset

| Column | Description |
|--------|-------------|
| `id` | Unique client identifier |
| `age` | Client's age: <br> <ul><li>`0`: 16-25</li><li>`1`: 26-39</li><li>`2`: 40-64</li><li>`3`: 65+</li></ul> |
| `gender` | Client's gender: <br> <ul><li>`0`: Female</li><li>`1`: Male</li></ul> |
| `driving_experience` | Years the client has been driving: <br> <ul><li>`0`: 0-9</li><li>`1`: 10-19</li><li>`2`: 20-29</li><li>`3`: 30+</li></ul> |
| `education` | Client's level of education: <br> <ul><li>`0`: No education</li><li>`1`: High school</li><li>`2`: University</li></ul> |
| `income` | Client's income level: <br> <ul><li>`0`: Poverty</li><li>`1`: Working class</li><li>`2`: Middle class</li><li>`3`: Upper class</li></ul> |
| `credit_score` | Client's credit score (between zero and one) |
| `vehicle_ownership` | Client's vehicle ownership status: <br><ul><li>`0`: Does not own their vehilce (paying off finance)</li><li>`1`: Owns their vehicle</li></ul> |
| `vehcile_year` | Year of vehicle registration: <br><ul><li>`0`: Before 2015</li><li>`1`: 2015 or later</li></ul> |
| `married` | Client's marital status: <br><ul><li>`0`: Not married</li><li>`1`: Married</li></ul> |
| `children` | Client's number of children |
| `postal_code` | Client's postal code | 
| `annual_mileage` | Number of miles driven by the client each year |
| `vehicle_type` | Type of car: <br> <ul><li>`0`: Sedan</li><li>`1`: Sports car</li></ul> |
| `speeding_violations` | Total number of speeding violations received by the client | 
| `duis` | Number of times the client has been caught driving under the influence of alcohol |
| `past_accidents` | Total number of previous accidents the client has been involved in |
| `outcome` | Whether the client made a claim on their car insurance (response variable): <br><ul><li>`0`: No claim</li><li>`1`: Made a claim</li></ul> |


```python
# Import required modules
import pandas as pd
import numpy as np
from statsmodels.formula.api import logit

# import data
cars=pd.read_csv("car_insurance.csv")
```


```python
# func for assessment
def accuracy(conf_mat):
    """find accuracy of logistic regression model. 
        Args: 
            conf_mat: confusion matrix
        Returns:
            float: accuracy of model"""
    tn = conf_mat[0,0]
    fp = conf_mat[0,1]
    fn = conf_mat[1,0]
    tp = conf_mat[1,1]
    acc = (tn+tp)/(tn+fp+fn+tp)
    return acc
    
def sensitivity(conf_mat):
    """find sensitivity of logistic regression model. 
        Args: 
            conf_mat: confusion matrix
        Returns:
            float: sensitivity of model"""
    tn = conf_mat[0,0]
    fp = conf_mat[0,1]
    fn = conf_mat[1,0]
    tp = conf_mat[1,1]
    sens = tp/(fn+tp)
    return sens
    
def specificity(conf_mat):
    """find specificity of logistic regression model. 
        Args: 
            conf_mat: confusion matrix
        Returns:
            float: specificity of model"""
    tn = conf_mat[0,0]
    fp = conf_mat[0,1]
    fn = conf_mat[1,0]
    tp = conf_mat[1,1]
    spec = tn/(tn+fp)
    return spec
```


```python
# 1. identify the best predictor of outcome
# age
mdl_age = logit("outcome~age", data=cars).fit()
explanatory_age = pd.DataFrame(
    {"age" : np.arange(0, 4)})
prediction_age = explanatory_age.assign(
    outcome = mdl_age.predict(explanatory_age))

conf_mat_age = mdl_age.pred_table()
print(accuracy(conf_mat_age))
print(sensitivity(conf_mat_age))
print(specificity(conf_mat_age))
```

    Optimization terminated successfully.
             Current function value: 0.511794
             Iterations 6
    0.7747
    0.46217682732205556
    0.9172855686617154



```python
cars.dtypes
```




    id                       int64
    age                      int64
    gender                   int64
    driving_experience      object
    education               object
    income                  object
    credit_score           float64
    vehicle_ownership      float64
    vehicle_year            object
    married                float64
    children               float64
    postal_code              int64
    annual_mileage         float64
    vehicle_type            object
    speeding_violations      int64
    duis                     int64
    past_accidents           int64
    outcome                float64
    dtype: object




```python
# gender
mdl_gender = logit("outcome~gender", data=cars).fit()
explanatory_gender = pd.DataFrame(
    {"gender" : np.arange(0, 3)})
prediction_gender = explanatory_gender.assign(
    outcome = mdl_gender.predict(explanatory_gender))

conf_mat_gender = mdl_gender.pred_table()
print(accuracy(conf_mat_gender))
print(sensitivity(conf_mat_gender))
print(specificity(conf_mat_gender))
```

    Optimization terminated successfully.
             Current function value: 0.615951
             Iterations 5
    0.6867
    0.0
    1.0



```python
# driving experience
cars["driving_experience"] = cars["driving_experience"].astype("category")
cars["driving_experience_nums"] = cars["driving_experience"].cat.codes
mdl_drivexp = logit("outcome ~ driving_experience_nums", data=cars).fit()
explanatory_drivexp = pd.DataFrame(
    {"driving_experience_nums" : np.arange(0, 4)})
prediction_drivexp = explanatory_drivexp.assign(
    outcome = mdl_drivexp.predict(explanatory_drivexp))

conf_mat_drivexp = mdl_drivexp.pred_table()
print(accuracy(conf_mat_drivexp))
drivexp_acc = accuracy(conf_mat_drivexp)
print(sensitivity(conf_mat_drivexp))
print(specificity(conf_mat_drivexp))
```

    Optimization terminated successfully.
             Current function value: 0.467390
             Iterations 7
    0.7771
    0.7076284711139483
    0.8087956895296344



```python
# education
cars["education"] = cars["education"].astype("category")
cars["education_nums"] = cars["education"].cat.codes

# model
mdl_edu = logit("outcome ~ education_nums", data=cars).fit()
explanatory_edu = pd.DataFrame(
    {"education_nums" : np.arange(0, 3)})
prediction_edu = explanatory_edu.assign(
    outcome = mdl_edu.predict(explanatory_edu))

# conf mat
conf_mat_edu = mdl_edu.pred_table()
print(accuracy(conf_mat_edu))
print(sensitivity(conf_mat_edu))
print(specificity(conf_mat_edu))
```

    Optimization terminated successfully.
             Current function value: 0.617408
             Iterations 5
    0.6867
    0.0
    1.0



```python
# income
cars["income"] = cars["income"].astype("category")
cars["income_nums"] = cars["income"].cat.codes

# model
mdl_income = logit("outcome ~ income_nums", data=cars).fit()
explanatory_income = pd.DataFrame(
    {"income_nums" : np.arange(0, 4)})
prediction_income = explanatory_income.assign(
    outcome = mdl_income.predict(explanatory_income))

# conf mat
conf_mat_income = mdl_income.pred_table()

print(accuracy(conf_mat_income))
print(sensitivity(conf_mat_income))
print(specificity(conf_mat_income))
```

    Optimization terminated successfully.
             Current function value: 0.620588
             Iterations 5
    0.6867
    0.0
    1.0



```python
# vehicle_ownership
cars["vehicle_ownership"] = cars["vehicle_ownership"].astype("category")
cars["vehicle_ownership_num"] = cars["vehicle_ownership"].cat.codes

# model
mdl_own = logit("outcome ~ vehicle_ownership_num", data=cars).fit()

# explanatory data
explanatory_own = pd.DataFrame(
    {"vehicle_ownership_num" : np.arange(0, 2)})

# predict
prediction_own = explanatory_own.assign(
    outcome = mdl_own.predict(explanatory_own))

# conf mat
conf_mat_own = mdl_own.pred_table()
print(accuracy(conf_mat_own))
print(sensitivity(conf_mat_own))
print(specificity(conf_mat_own))
```

    Optimization terminated successfully.
             Current function value: 0.552412
             Iterations 5
    0.7351
    0.5608043408873284
    0.8146206494830348



```python
# vehicle_year
cars["vehicle_year"] = cars["vehicle_year"].astype("category")
cars["vehicle_year_num"] = cars["vehicle_year"].cat.codes

# model
mdl_year = logit("outcome ~ vehicle_year_num", data=cars).fit()

# explanatory data
explanatory_year = pd.DataFrame(
    {"vehicle_year_num" : np.arange(0, 2)})

# predict
prediction_year = explanatory_year.assign(
    outcome = mdl_year.predict(explanatory_year))

# conf mat
conf_mat_year = mdl_year.pred_table()

# quantify fit
print(accuracy(conf_mat_year))
print(sensitivity(conf_mat_year))
print(specificity(conf_mat_year))
```

    Optimization terminated successfully.
             Current function value: 0.572668
             Iterations 6
    0.6867
    0.0
    1.0



```python
# married
cars["married"] = cars["married"].astype("category")
cars["married_num"] = cars["married"].cat.codes

# model
mdl_marry = logit("outcome ~ married_num", data=cars).fit()

# explanatory data
explanatory_marry = pd.DataFrame(
    {"married_num" : np.arange(0, 2)})

# predict
prediction_marry = explanatory_marry.assign(
    outcome = mdl_marry.predict(explanatory_marry))

# conf mat
conf_mat_marry = mdl_marry.pred_table()

# quantify fit
print(accuracy(conf_mat_marry))
print(sensitivity(conf_mat_marry))
print(specificity(conf_mat_marry))
```

    Optimization terminated successfully.
             Current function value: 0.586659
             Iterations 5
    0.6867
    0.0
    1.0



```python
# postal_code
cars["postal_code"] = cars["vehicle_year"].astype("category")
cars["vehicle_year_num"] = cars["vehicle_year"].cat.codes

# model
mdl_year = logit("outcome ~ vehicle_year_num", data=cars).fit()

# explanatory data
explanatory_year = pd.DataFrame(
    {"vehicle_year_num" : np.arange(0, 2)})

# predict
prediction_year = explanatory_year.assign(
    outcome = mdl_year.predict(explanatory_year))

# conf mat
conf_mat_year = mdl_year.pred_table()

# quantify fit
print(accuracy(conf_mat_year))
print(sensitivity(conf_mat_year))
print(specificity(conf_mat_year))
```

    Optimization terminated successfully.
             Current function value: 0.572668
             Iterations 6
    0.6867
    0.0
    1.0



```python
# speeding violations
cars["speeding_violations"].max()

# model
mdl_speed = logit("outcome ~ speeding_violations", data=cars).fit()

# explanatory data
explanatory_speed = pd.DataFrame(
    {"speeding_violations" : np.arange(0, 23)})

# predict
prediction_speed = explanatory_speed.assign(
    outcome = mdl_speed.predict(explanatory_speed))

# conf matrix
conf_mat_speed = mdl_speed.pred_table()

# quantify fit
print(accuracy(conf_mat_speed))
print(sensitivity(conf_mat_speed))
print(specificity(conf_mat_speed))
```

    Optimization terminated successfully.
             Current function value: 0.558922
             Iterations 7
    0.6867
    0.0
    1.0



```python
# vehicle_type
```


```python
# duis

# model
mdl_dui = logit("outcome ~ duis", data=cars).fit()

# explanatory data
explanatory_dui = pd.DataFrame(
    {"duis" : np.arange(0, 7)})

# predict
prediction_dui = explanatory_dui.assign(
    outcome = mdl_dui.predict(explanatory_dui))

# conf matrix
conf_mat_dui = mdl_dui.pred_table()

# quantify fit
print(accuracy(conf_mat_dui))
print(sensitivity(conf_mat_dui))
print(specificity(conf_mat_dui))
```

    Optimization terminated successfully.
             Current function value: 0.598699
             Iterations 6
    0.6867
    0.0
    1.0



```python
# past_accidents

# model
mdl_accid = logit("outcome ~ past_accidents", data=cars).fit()

# explanatory data
explanatory_accid = pd.DataFrame(
    {"past_accidents" : np.arange(0, 15)})

# predict
prediction_accid = explanatory_accid.assign(
    outcome = mdl_accid.predict(explanatory_accid))

# conf matrix
conf_mat_accid = mdl_accid.pred_table()

# quantify fit
print(accuracy(conf_mat_accid))
print(sensitivity(conf_mat_accid))
print(specificity(conf_mat_accid))
```

    Optimization terminated successfully.
             Current function value: 0.549220
             Iterations 7
    0.6867
    0.0
    1.0



```python
# annual_mileage

# model
mdl_mileage = logit("outcome ~ annual_mileage", data=cars).fit()

# explanatory data
explanatory_mileage = pd.DataFrame(
    {"annual_mileage" : np.arange(0, 22000)})

# predict
prediction_mileage = explanatory_mileage.assign(
    outcome = mdl_mileage.predict(explanatory_mileage))

# conf matrix
conf_mat_mileage = mdl_mileage.pred_table()

# quantify fit
print(accuracy(conf_mat_mileage))
print(sensitivity(conf_mat_mileage))
print(specificity(conf_mat_mileage))
```

    Optimization terminated successfully.
             Current function value: 0.601906
             Iterations 5
    0.6933539754506248
    0.03665480427046263
    0.9894111984598107



```python
# credit_score
# model
mdl_cred = logit("outcome ~ credit_score", data=cars).fit()

# explanatory data
explanatory_cred = pd.DataFrame(
    {"credit_score" : np.linspace(0, 1)})

# predict
prediction_cred = explanatory_cred.assign(
    outcome = mdl_cred.predict(explanatory_cred))

# conf matrix
conf_mat_cred = mdl_cred.pred_table()

# quantify fit
print(accuracy(conf_mat_cred))
print(sensitivity(conf_mat_cred))
print(specificity(conf_mat_cred))
```

    Optimization terminated successfully.
             Current function value: 0.567469
             Iterations 6
    0.7066977156797516
    0.25556733828207845
    0.91291000161577



```python
# save

best_feature_df = pd.DataFrame(
    {"best_feature": ["driving_experience"], 
     "best_accuracy": [drivexp_acc]})
```
