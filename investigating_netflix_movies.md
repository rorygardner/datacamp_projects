<center><img src="redpopcorn.jpg"></center>

**Netflix**! What started in 1997 as a DVD rental service has since exploded into one of the largest entertainment and media companies.

Given the large number of movies and series available on the platform, it is a perfect opportunity to flex your exploratory data analysis skills and dive into the entertainment industry.

You work for a production company that specializes in nostalgic styles. You want to do some research on movies released in the 1990's. You'll delve into Netflix data and perform exploratory data analysis to better understand this awesome movie decade!

You have been supplied with the dataset `netflix_data.csv`, along with the following table detailing the column names and descriptions. Feel free to experiment further after submitting!

## The data
### **netflix_data.csv**
| Column | Description |
|--------|-------------|
| `show_id` | The ID of the show |
| `type` | Type of show |
| `title` | Title of the show |
| `director` | Director of the show |
| `cast` | Cast of the show |
| `country` | Country of origin |
| `date_added` | Date added to Netflix |
| `release_year` | Year of Netflix release |
| `duration` | Duration of the show in minutes |
| `description` | Description of the show |
| `genre` | Show genre |


```python
# Importing pandas and matplotlib
import pandas as pd
import matplotlib.pyplot as plt

# Read in the Netflix CSV as a DataFrame
netflix_df = pd.read_csv("netflix_data.csv")
```


```python
# nmv - only 90's movies
nmv = netflix_df[(netflix_df["type"] == "Movie") &
                (netflix_df["release_year"] >= 1990) &
                (netflix_df["release_year"] <= 1999)]

#find mode
duration = int(nmv["duration"].value_counts().idxmax())
print(str("duration: ") + str(duration))

#nmv_sam - <90 min, action, 90's movies
nmv_sam = nmv[(nmv["genre"] == "Action") &
                (nmv["duration"] < 90)]

#count short action movies
short_movie_count = len(nmv_sam)
print(str("short_movie_count: ") + str(short_movie_count))


```

    duration: 94
    short_movie_count: 7

