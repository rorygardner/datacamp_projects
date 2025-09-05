![Two data scientists working on a dashboard.](hr-image-small.png)

A common problem when creating models to generate business value from data is that the datasets can be so large that it can take days for the model to generate predictions. Ensuring that your dataset is stored as efficiently as possible is crucial for allowing these models to run on a more reasonable timescale without having to reduce the size of the dataset.

You've been hired by a major online data science training provider called *Training Data Ltd.* to clean up one of their largest customer datasets. This dataset will eventually be used to predict whether their students are looking for a new job or not, information that they will then use to direct them to prospective recruiters.

You've been given access to `customer_train.csv`, which is a subset of their entire customer dataset, so you can create a proof-of-concept of a much more efficient storage solution. The dataset contains anonymized student information, and whether they were looking for a new job or not during training:

| Column                   | Description                                                                      |
|------------------------- |--------------------------------------------------------------------------------- |
| `student_id`             | A unique ID for each student.                                                    |
| `city`                   | A code for the city the student lives in.                                        |
| `city_development_index` | A scaled development index for the city.                                         |
| `gender`                 | The student's gender.                                                            |
| `relevant_experience`    | An indicator of the student's work relevant experience.                          |
| `enrolled_university`    | The type of university course enrolled in (if any).                              |
| `education_level`        | The student's education level.                                                   |
| `major_discipline`       | The educational discipline of the student.                                       |
| `experience`             | The student's total work experience (in years).                                  |
| `company_size`           | The number of employees at the student's current employer.                       |
| `company_type`           | The type of company employing the student.                                       |
| `last_new_job`           | The number of years between the student's current and previous jobs.             |
| `training_hours`         | The number of hours of training completed.                                       |
| `job_change`             | An indicator of whether the student is looking for a new job (`1`) or not (`0`). |


```python
# Import necessary libraries
import pandas as pd

# Load the dataset
ds_jobs = pd.read_csv("customer_train.csv")

# View the dataset
ds_jobs.head()
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
      <th>student_id</th>
      <th>city</th>
      <th>city_development_index</th>
      <th>gender</th>
      <th>relevant_experience</th>
      <th>enrolled_university</th>
      <th>education_level</th>
      <th>major_discipline</th>
      <th>experience</th>
      <th>company_size</th>
      <th>company_type</th>
      <th>last_new_job</th>
      <th>training_hours</th>
      <th>job_change</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>8949</td>
      <td>city_103</td>
      <td>0.920</td>
      <td>Male</td>
      <td>Has relevant experience</td>
      <td>no_enrollment</td>
      <td>Graduate</td>
      <td>STEM</td>
      <td>&gt;20</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>1</td>
      <td>36</td>
      <td>1.0</td>
    </tr>
    <tr>
      <th>1</th>
      <td>29725</td>
      <td>city_40</td>
      <td>0.776</td>
      <td>Male</td>
      <td>No relevant experience</td>
      <td>no_enrollment</td>
      <td>Graduate</td>
      <td>STEM</td>
      <td>15</td>
      <td>50-99</td>
      <td>Pvt Ltd</td>
      <td>&gt;4</td>
      <td>47</td>
      <td>0.0</td>
    </tr>
    <tr>
      <th>2</th>
      <td>11561</td>
      <td>city_21</td>
      <td>0.624</td>
      <td>NaN</td>
      <td>No relevant experience</td>
      <td>Full time course</td>
      <td>Graduate</td>
      <td>STEM</td>
      <td>5</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>never</td>
      <td>83</td>
      <td>0.0</td>
    </tr>
    <tr>
      <th>3</th>
      <td>33241</td>
      <td>city_115</td>
      <td>0.789</td>
      <td>NaN</td>
      <td>No relevant experience</td>
      <td>NaN</td>
      <td>Graduate</td>
      <td>Business Degree</td>
      <td>&lt;1</td>
      <td>NaN</td>
      <td>Pvt Ltd</td>
      <td>never</td>
      <td>52</td>
      <td>1.0</td>
    </tr>
    <tr>
      <th>4</th>
      <td>666</td>
      <td>city_162</td>
      <td>0.767</td>
      <td>Male</td>
      <td>Has relevant experience</td>
      <td>no_enrollment</td>
      <td>Masters</td>
      <td>STEM</td>
      <td>&gt;20</td>
      <td>50-99</td>
      <td>Funded Startup</td>
      <td>4</td>
      <td>8</td>
      <td>0.0</td>
    </tr>
  </tbody>
</table>
</div>




```python
# Create a copy of ds_jobs for transforming
ds_jobs_transformed = ds_jobs.copy()

# explore
print(ds_jobs_transformed.nunique())
print(ds_jobs_transformed.dtypes)
print(ds_jobs_transformed.memory_usage())


```

    student_id                19158
    city                        123
    city_development_index       93
    gender                        3
    relevant_experience           2
    enrolled_university           3
    education_level               5
    major_discipline              6
    experience                   22
    company_size                  8
    company_type                  6
    last_new_job                  6
    training_hours              241
    job_change                    2
    dtype: int64
    student_id                  int64
    city                       object
    city_development_index    float64
    gender                     object
    relevant_experience        object
    enrolled_university        object
    education_level            object
    major_discipline           object
    experience                 object
    company_size               object
    company_type               object
    last_new_job               object
    training_hours              int64
    job_change                float64
    dtype: object
    Index                        128
    student_id                153264
    city                      153264
    city_development_index    153264
    gender                    153264
    relevant_experience       153264
    enrolled_university       153264
    education_level           153264
    major_discipline          153264
    experience                153264
    company_size              153264
    company_type              153264
    last_new_job              153264
    training_hours            153264
    job_change                153264
    dtype: int64



```python
### dtypes
## 1. two factors = bool
# relevant_experience, job_change
ds_jobs_transformed = ds_jobs_transformed.astype({
    "relevant_experience":bool, "job_change":bool
})

## 2. integers = int32
# student_id, training_hours
ds_jobs_transformed = ds_jobs_transformed.astype({
    col:'int32' for col in ds_jobs_transformed.select_dtypes(include=['int64']).columns
})

## 3. float = float16
ds_jobs_transformed = ds_jobs_transformed.astype({
    col:'float16' for col in ds_jobs_transformed.select_dtypes(include=['float64']).columns
})
## 4. nominal = category
# gender, enrolled_university, major_discipline, company_type
ds_jobs_transformed = ds_jobs_transformed.astype({
    "city":"category",
    "gender":"category", 
    "enrolled_university":"category",
    "major_discipline":"category", 
    "company_type":"category"
})

## 5. ordinal = ordered category
# education_level, experience, company size, last_new_job

# edu
edu_og_vals = ds_jobs_transformed["education_level"].value_counts()
edu_levels = ['Primary School', 'High School', 'Graduate', 'Masters', 'PhD']
ds_jobs_transformed["education_level"]= ds_jobs_transformed["education_level"].astype("category").cat.set_categories(edu_levels, ordered=True)


# company size
ds_jobs_transformed['company_size'].value_counts()
size_categories = ['<10', '10-49', '50-99', '100-499',
                   '500-999', '1000-4999', '5000-9999', '10000+']
ds_jobs_transformed["company_size"] = ds_jobs_transformed["company_size"].astype("category").cat.set_categories(size_categories,ordered=True)

# last_new_job
ds_jobs_transformed['last_new_job'].value_counts()
last_categories=['never', '1', '2', '3', '4', '>4']
ds_jobs_transformed["last_new_job"] = ds_jobs_transformed["last_new_job"].astype("category").cat.set_categories(last_categories,ordered=True)

# enrolled
ds_jobs_transformed['enrolled_university'].value_counts()
enroll_cats = ['no_enrollment', 'Part time course', 'Full time course']
ds_jobs_transformed['enrolled_university'] = ds_jobs_transformed['enrolled_university'].astype("category")
ds_jobs_transformed['enrolled_university'] = ds_jobs_transformed['enrolled_university'].cat.set_categories(enroll_cats, ordered=True)

```


```python
## filtering
# 10+ years, 1000+ employees
xp_map = {"<1": 0, ">20": 21}
for i in range(1, 21):
    xp_map[str(i)] = i

# Map experience and handle NaN values
ds_jobs_transformed['experience'] = ds_jobs_transformed['experience'].map(xp_map)
ds_jobs_transformed['experience'] = ds_jobs_transformed['experience'].fillna(-1).astype(int)

# Filter the DataFrame
ds_jobs_transformed = ds_jobs_transformed[ds_jobs_transformed['experience'] >= 10]
ds_jobs_transformed = ds_jobs_transformed[ds_jobs_transformed['company_size'].isin(["1000-4999", "5000-9999", "10000+"])]
```


```python
# put back
ordered_xp = [str(i) for i in range(10, 22)]
ds_jobs_transformed['experience'] = ds_jobs_transformed['experience'].astype("category")
ds_jobs_transformed['experience'] = ds_jobs_transformed['experience'].cat.set_categories(ordered_xp, ordered=True)
ds_jobs_transformed['experience'] = ds_jobs_transformed['experience'].cat.rename_categories({"21":">20"})

ds_jobs_transformed.dtypes

```




    student_id                   int32
    city                      category
    city_development_index     float16
    gender                    category
    relevant_experience           bool
    enrolled_university       category
    education_level           category
    major_discipline          category
    experience                category
    company_size              category
    company_type              category
    last_new_job              category
    training_hours               int32
    job_change                    bool
    dtype: object


