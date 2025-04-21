# Introduction

### Topic

This page details my investigation of a portion of the **[Food.com Recipes and Interactions](https://www.kaggle.com/datasets/shuyangli94/food-com-recipes-and-user-interactions/data)** dataset. 
I'm attempting to answer the question:
> "What kinds of recipes get high reviews?". 

I hope my analysis will reveal ideas about what kinds of recipes get rated highly, which may help writers create better performing recipes, as well as readers to recognize biases in others' reviews.

### Dataset Details

This project technically covers two datasets, described below

- The **recipes** dataset contains 83,782 rows and 12 columns. The dataset contains these relevant columns. 

| Column        | Type    | Description                        |
|:--------------|:--------|:-----------------------------------|
| minutes       | Integer | Minutes to prepare recipe          |
| n_steps       | Integer | Number of steps in recipe          |
| n_ingredients | Integer | Number of ingredients in recipe    |
| step          | String  | Text for recipe steps, in order                        |

- The **reviews** dataset contains 731,927 rows and 5 columns. The dataset contains these relevant columns.

| Column        | Type    | Description       |
|:--------------|:--------|:------------------|
| rating       | Integer | Rating given; 1-5 |

# Data Cleaning and Exploratory Data Analysis

### Data Cleaning

To work on my main question, I needed to use the ratings dataset to get each recipe's average rating.
I combined the two datasets by left merging recipes and reviews. 
I then grouped the merged dataset by recipe, and I applied dataframe operations to create columns for the mean rating and rating
count. 

I am now working with a single dataframe that looks just like the **recipes** dataset, but with these new columns.

| Column       | Type  | Description                             |
|:-------------|:------|:----------------------------------------|
| review_count | Float | Total number of reviews for this recipe |
| avg_rating   | Float | Average of all ratings for this recipe  |

I plotted the distribution of ratings, and noticed sharp peaks at each of the whole number reviews. 
This clued me in that *many* recipes only had a handful of reviews. 
For my purposes, I want to analyse recipes that have been rated more fairly, so I elected to drop recipes that had less than 5 reviews.

I also decided to drop recipes that took longer than 520 minutes or had more than 40 steps, to avoid extreme outliers that could harm my results.

I later tested without these restrictions and got very poor results. 
It's safe to say that my analysis was only possible with these exclusions. 

This is the head of the cleaned dataset, with just the relevant columns. In total, it has 11,026 rows.

| minutes | n_steps | n_ingredients | steps                                                                                                                                 | review_count | avg_rating |
|---------|----|----|---------------------------------------------------------------------------------------------------------------------------------------|----|----|
| 75      | 6 | 6 | [\'in a bowl combine eggs , sugar , flour and milk\', \'grease a stoneware dish with...]                                              | 6 | 4.33333 |
| 5       | 2 | 4       | [\'just mix together ouzo and orange juice and pour over ice in a collins glass\', \'garnish with an orange slice and get friendly\'] | 10      | 4.8 |
| 40      | 8 |  5      | [\'preheat oven to 350 degrees\', \'in mixing bowl , first combine the sour cream and vegetable soup mix...]                          |  12     | 4.83333 |
| 5       | 6 | 6       | [\'in a blender combine coffee with ice cream and chocolate syrup\', \'blend until smooth\', \'add in sugar...]                       | 8       | 5 |
| 50 | 12 | 13      | [\'to make the fish: place the fish in a shallow baking dish and sprinkle with the lime juice , paprika , salt...]                    | 5       | 4 |

### Univariate Analysis

Embedded below is a plot of the distribution of average ratings for my cleaned dataset.

<iframe src="assets/ratings_distribution.html" width="800" height="600" frameborder="0"> </iframe>

Notice that the vast majority of the average reviews are clustered in the 4-5 range. 
We can also still see some spikes at the whole numbers, which were even more pronounced before cleaning.


### Bivariate Analysis



### Interesting Aggregates



### Imputation

To be cautious, all ratings with a value of **0** were replaced with **NaN**, and all rows with a **NaN** rating were excluded. 
However, it turns out that there were no **0** or **NaN** values in the portion of the recipes dataset that I used. 

# Framing a Prediction Problem



# Baseline Model



# Final Model



