![NYC Skyline](nyc.jpg)

Welcome to New York City, one of the most-visited cities in the world. There are many Airbnb listings in New York City to meet the high demand for temporary lodging for travelers, which can be anywhere between a few nights to many months. In this project, we will take a closer look at the New York Airbnb market by combining data from multiple file types like `.csv`, `.tsv`, and `.xlsx`.

Recall that **CSV**, **TSV**, and **Excel** files are three common formats for storing data. 
Three files containing data on 2019 Airbnb listings are available to you:

**data/airbnb_price.csv**
This is a CSV file containing data on Airbnb listing prices and locations.
- **`listing_id`**: unique identifier of listing
- **`price`**: nightly listing price in USD
- **`nbhood_full`**: name of borough and neighborhood where listing is located

**data/airbnb_room_type.xlsx**
This is an Excel file containing data on Airbnb listing descriptions and room types.
- **`listing_id`**: unique identifier of listing
- **`description`**: listing description
- **`room_type`**: Airbnb has three types of rooms: shared rooms, private rooms, and entire homes/apartments

**data/airbnb_last_review.tsv**
This is a TSV file containing data on Airbnb host names and review dates.
- **`listing_id`**: unique identifier of listing
- **`host_name`**: name of listing host
- **`last_review`**: date when the listing was last reviewed


```python
# Import necessary packages
import pandas as pd
import numpy as np

prices = pd.read_csv('data/airbnb_price.csv')
rooms = pd.read_excel('data/airbnb_room_type.xlsx')
reviews = pd.read_csv('data/airbnb_last_review.tsv', sep='\t')
```


```python
### dates of earliest and most recent reviews
reviews['last_review_dt'] = pd.to_datetime(reviews['last_review'])

first_reviewed = reviews['last_review_dt'].min()
last_reviewed = reviews['last_review_dt'].max()
```


```python
### private rooms count
from thefuzz import fuzz
from thefuzz import process

# category list
type_cats = ['shared room', 'private room', 'entire home/apt']

for roomtype in type_cats: 
    p_matches = process.extract(roomtype, rooms['room_type'], limit = rooms.shape[0])
    for match in p_matches: 
        if match[1] >=80: 
            rooms.loc[rooms['room_type'] == match[0], 'room_type'] = roomtype

nb_private_rooms = rooms.loc[rooms['room_type'] == 'private room', 'room_type'].count()
```


```python
### avg listing price

prices['price_num'] = prices['price'].str.replace("dollars", "")
prices['price_num'] = prices['price_num'].str.strip()
prices['price_num'] = prices['price_num'].astype(int)

avg_price = round(prices['price_num'].mean(), 2)

```


```python
### assemble
dict_reviews = {'first_reviewed':[first_reviewed], 
               'last_reviewed':[last_reviewed], 
               'nb_private_rooms':[nb_private_rooms], 
               'avg_price':[avg_price]}

review_dates = pd.DataFrame(dict_reviews)
```
