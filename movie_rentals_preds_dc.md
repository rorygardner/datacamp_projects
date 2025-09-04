![dvd_image](dvd_image.jpg)

A DVD rental company needs your help! They want to figure out how many days a customer will rent a DVD for based on some features and has approached you for help. They want you to try out some regression models which will help predict the number of days a customer will rent a DVD for. The company wants a model which yeilds a MSE of 3 or less on a test set. The model you make will help the company become more efficient inventory planning.

The data they provided is in the csv file `rental_info.csv`. It has the following features:
- `"rental_date"`: The date (and time) the customer rents the DVD.
- `"return_date"`: The date (and time) the customer returns the DVD.
- `"amount"`: The amount paid by the customer for renting the DVD.
- `"amount_2"`: The square of `"amount"`.
- `"rental_rate"`: The rate at which the DVD is rented for.
- `"rental_rate_2"`: The square of `"rental_rate"`.
- `"release_year"`: The year the movie being rented was released.
- `"length"`: Lenght of the movie being rented, in minuites.
- `"length_2"`: The square of `"length"`.
- `"replacement_cost"`: The amount it will cost the company to replace the DVD.
- `"special_features"`: Any special features, for example trailers/deleted scenes that the DVD also has.
- `"NC-17"`, `"PG"`, `"PG-13"`, `"R"`: These columns are dummy variables of the rating of the movie. It takes the value 1 if the move is rated as the column name and 0 otherwise. For your convinience, the reference dummy has already been dropped.


```python
import pandas as pd
import numpy as np

from sklearn.model_selection import train_test_split
from sklearn.metrics import mean_squared_error as MSE
from sklearn.ensemble import RandomForestRegressor
from sklearn.model_selection import GridSearchCV


# Import any additional modules and start coding below
rentals = pd.read_csv("rental_info.csv")

# dtypes
rentals['rental_date'] = pd.to_datetime(rentals['rental_date'])
rentals['return_date'] = pd.to_datetime(rentals['return_date'])
rentals['special_features'] = rentals['special_features'].astype('str')

# rental_length_days
rentals['rental_length_days'] = (rentals['return_date']-rentals['rental_date']).dt.days

# dummies: deleted scenes/bts
rentals['deleted_scenes'] = (rentals['special_features'].str.contains('Deleted Scenes', na=False)).astype('int')
rentals['behind_the_scenes'] = (rentals['special_features'].str.contains('Behind the Scenes', na=False)).astype('int')


# X and y
X = rentals.drop(["rental_date", "return_date", "special_features", "rental_length_days"], axis = 1)
y = rentals['rental_length_days']

# split
X_train, X_test, y_train, y_test = train_test_split(X, y, test_size = 0.2, random_state=9)

# dt
rf = RandomForestRegressor(random_state=9)

# params 
params_rf = {"n_estimators":[50,100,250], 
            "max_depth":[None, 5, 10, 15, 20], 
            "max_features":["sqrt", "log2"],
            "min_samples_split":[2,5,10], 
            "min_samples_leaf":[1,2,4]}

# grid search
grid_rf = GridSearchCV(estimator=rf, 
                      param_grid=params_rf, 
                      cv=5, 
                      scoring="neg_mean_squared_error", 
                      verbose=1, 
                      n_jobs=-1)

# fit
grid_rf.fit(X_train, y_train)

best_score = grid_rf.best_score_
best_params = grid_rf.best_params_
best_model = grid_rf.best_estimator_
print(best_score, best_params)

y_pred = best_model.predict(X_test)
best_mse = MSE(y_test, y_pred)
print(best_mse)
```

    Fitting 5 folds for each of 270 candidates, totalling 1350 fits
    -1.9769571986752568 {'max_depth': 15, 'max_features': 'sqrt', 'min_samples_leaf': 1, 'min_samples_split': 2, 'n_estimators': 250}
    1.9758768942854525



```python

```




    rental_date           datetime64[ns, UTC]
    return_date           datetime64[ns, UTC]
    amount                            float64
    release_year                      float64
    rental_rate                       float64
    length                            float64
    replacement_cost                  float64
    special_features                   object
    NC-17                               int64
    PG                                  int64
    PG-13                               int64
    R                                   int64
    amount_2                          float64
    length_2                          float64
    rental_rate_2                     float64
    rental_length_days                  int64
    deleted_scenes                      int64
    behind_the_scenes                   int64
    dtype: object


