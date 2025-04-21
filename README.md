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

I plotted the distribution of ratings, and noticed sharp peaks at each of the whole number reviews. 
This clued me in that *many* recipes only had a handful of reviews. 
For my purposes, I want to analyse recipes that have been rated more fairly, so I elected to drop recipes that had less than 5 reviews.

I also decided to drop recipes that took longer than 520 minutes or had more than 40 steps, to avoid extreme outliers that could harm my results.

I later tested without these restrictions and got very poor results. 
It's safe to say that my analysis was only possible with these exclusions. 

This is the head of the cleaned dataset.

| name | id | minutes | contributor_id | submitted | tags | nutrition | n_steps | steps | description | ingredients |
n_ingredients | avg_rating | review_count
|\n|:--------------------------------------|-------:|----------:|-----------------:|:------------|:-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|:-----------------------------------------------|----------:|:-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|:-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|:------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|----------------:|-------------:|---------------:
|\n| puddingkuchen custard bake | 353171 | 75 | 573325 |
2009-01-31 | [\'time-to-make\', \'course\', \'main-ingredient\', \'preparation\', \'occasion\', \'desserts\', \'eggs-dairy\', \'fruit\', \'easy\', \'beginner-cook\', \'vegetarian\', \'dietary\', \'low-sodium\', \'comfort-food\', \'low-in-something\', \'brunch\', \'taste-mood\', \'sweet\', \'4-hours-or-less\']                                                                                                                                                                  | [1582.6, 88.0, 402.0, 27.0, 96.0, 156.0, 73.0] |
6 | [\'in a bowl combine eggs , sugar , flour and milk\', \'grease a stoneware dish with all the butter\', \'pour in the dough and add fruit , raisins or almonds to taste\', \'bake in the cold oven at 180c for about 60 minutes or until custard has set\', \'you might need to cover the dish after 40 minutes or so\', \'allow to stand and set for at least 10 minutes before serving\']                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                               |
this recipe comes from belgium. it is a lovely custard fruit bake, which resembles a cake. can be eaten warm or cold
served with vanilla ice-cream or just
plain. | [\'eggs\', \'sugar\', \'flour\', \'milk\', \'butter\', \'fruit\']                                                                                                                         |
6 | 4.33333 | 6 |\n| | | | | | | | | | ive never used frozen berries in this. you can try but then make sure that they
are not thawed or your custard won\'t set. | | | | |\n| sexy greek cocktail | 423875 | 5 | 65502 |
2010-05-07 | [\'15-minutes-or-less\', \'time-to-make\', \'course\', \'main-ingredient\', \'cuisine\', \'preparation\', \'for-1-or-2\', \'5-ingredients-or-less\', \'beverages\', \'fruit\', \'greek\', \'easy\', \'european\', \'cocktails\', \'citrus\', \'oranges\', \'number-of-servings\', \'3-steps-or-less\']                                                                                                                                                                   | [94.7, 0.0, 70.0, 0.0, 2.0, 0.0, 7.0]          |
2 | [\'just mix together ouzo and orange juice and pour over ice in a collins glass\', \'garnish with an orange slice and get friendly\']                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                            |
the greek lady that made this recipe said that there are lots of sexy greeks in her country! this cocktail will help you
get friendly with them! :)
enjoy!                                                                                                                                                                                                                                                                                     | [\'ouzo\', \'orange juice\', \'ice cube\', \'orange slice\']                                                                                                                          |
4 | 4.8 | 10 |\n| 5 things hot mexican green chile dip | 315537 | 40 | 899120 |
2008-07-24 | [\'60-minutes-or-less\', \'time-to-make\', \'course\', \'main-ingredient\', \'cuisine\', \'preparation\', \'north-american\', \'5-ingredients-or-less\', \'appetizers\', \'eggs-dairy\', \'vegetables\', \'mexican\', \'easy\', \'dips\', \'cheese\', \'dietary\', \'high-calcium\', \'low-carb\', \'inexpensive\', \'high-in-something\', \'low-in-something\', \'peppers\', \'tomatoes\']                                                                                        | [377.8, 48.0, 15.0, 51.0, 29.0, 96.0, 3.0]     |
8 | [\'preheat oven to 350 degrees\', \'in mixing bowl , first combine the sour cream and vegetable soup mix\', \'then drain the excess liquid off of the rotel and green chiles and stir both cans in with the sour cream mixture\', \'lastly , add 2 cups of the cheese and stir everything together\', \'pour mixture into a 10" round baking dish , top with additional 1 / 2 cup or more of cheese\', \'bake for 30 minutes\', \'allow the dish to cool for at least 20 minutes before serving with your favorite tortilla or corn chips\', \'happy dipping !\']                                                                                                                                                                                                                                                                                                                                                            |
i take this dip to almost every gathering i go to.....and have never had to haul any of it home!  it\'s never had a real
name, but when people ask what\'s in it?/how do you make it?, the response is always the same:  "it\'s so easy. five
things and
stir!". | [\'sour cream\', \'cheddar cheese\', \'tomatoes and green chilies\', \'diced green chilies\', \'knorr vegetable soup mix\']                                                             |
5 | 4.83333 | 12 |\n| | | | | | | | | | | | | | |\n| | | | | | | | | | you can actually add additional items like diced
jalapenos, sliced black olives, etc to make it your own.....but the basic recipe is always a hit!  i use the extra hot
rotel to kick it up, but for those who prefer milder flavor, you can use the regular or even \'mild\' rotel. i do highly
recommend rotel (rather than any other brand of diced tomatoes w/chilis)...there\'s just something about the flavor
that\'s very unique. enjoy! | | | | |\n| almost starbucks frappuccino | 289671 | 5 | 89831 |
2008-03-02 | [\'15-minutes-or-less\', \'time-to-make\', \'course\', \'main-ingredient\', \'preparation\', \'occasion\', \'for-1-or-2\', \'low-protein\', \'beverages\', \'eggs-dairy\', \'easy\', \'refrigerator\', \'beginner-cook\', \'summer\', \'chocolate\', \'smoothies\', \'food-processor-blender\', \'dietary\', \'low-sodium\', \'low-cholesterol\', \'seasonal\', \'shakes\', \'low-calorie\', \'low-carb\', \'low-in-something\', \'equipment\', \'small-appliance\', \'number-of-servings\'] | [313.0, 19.0, 136.0, 8.0, 10.0, 35.0, 15.0]    |
6 | [\'in a blender combine coffee with ice cream and chocolate syrup\', \'blend until smooth\', \'add in sugar and blend slightly until combined\', \'place about 1 cup crushed ice in two large tall glasses\', \'divide the mixture into the glasses\', \'top with a dollup of whipped cream\']                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                           |
since i serve this is very large glasses i have listed at two servings, you may divide into as many servings as you
wish, make certain to use very strong coffee for this --- this is very
good!                                                                                                                                                                                                                                               | [\'strong coffee\', \'chocolate ice cream\', \'chocolate syrup\', \'sugar\', \'ice\', \'whipped cream\']                                                                                  |
6 | 5 | 8 |\n| biggest loser fish tacos | 392158 | 50 | 280271 |
2009-09-28 | [\'60-minutes-or-less\', \'time-to-make\', \'course\', \'main-ingredient\', \'cuisine\', \'preparation\', \'north-american\', \'healthy\', \'lunch\', \'seafood\', \'mexican\', \'fish\', \'baja\', \'dietary\', \'low-cholesterol\', \'sandwiches\', \'healthy-2\', \'low-in-something\', \'equipment\', \'grilling\']                                                                                                                                                      | [323.5, 13.0, 9.0, 32.0, 40.0, 12.0, 13.0]     |
12 | [\'to make the fish: place the fish in a shallow baking dish and sprinkle with the lime juice , paprika , salt , black pepper , and chill powder\', \'cover , refrigerate , and marinate for about 30 minutes\', \'preheat the grill to medium-high heat\', \'lightly coat a 24\\\' x 12" piece of foil with olive oil cooking spray\', \'place the fish in a single layer in the center of the foil\', \'fold the foil over the and fold the ends upward to seal in the fish\', \'place the foil packet on the preheated grill\', \'cook for 7 to 10 minutes , or until the fish is opaque\', \'remove from the grill\', \'to assemble the tacos: warp the tortillas in foil and place them on the grill to warm for 2 minutes\', \'spread about one-eighth of the mashed avocado on each tortilla and top with one-eight of the the fish\', \'sprinkle each with cheese , salsa , cilantro , cabbage , and hot sauce , if desired\'] |
this is out of the "biggest loser"
cookbook | [\'fish fillets\', \'lime juice\', \'paprika\', \'salt\', \'ground black pepper\', \'chili powder\', \'tortillas\', \'avocado\', \'cheese\', \'salsa\', \'fresh cilantro\', \'cabbage\', \'hot sauce\'] |
13 | 4 | 5 |\n| | | | | | | | | | 140 calories, 18 g protein, 13 g carbohydrates, 5 g fat, 30 mg cholesterol, 8 g fiber,
480 mg sodium. please take the advice from sjmittens and increase the spices, especially if you enjoy spicy
food. | | | | |

### Univariate Analysis



### Bivariate Analysis



### Interesting Aggregates



### Imputation

To be cautious, all ratings with a value of **0** were replaced with **NaN**, and all rows with a **NaN** rating were excluded. 
However, it turns out that there were no **0** or **NaN** values in the portion of the recipes dataset that I used. 

# Framing a Prediction Problem



# Baseline Model



# Final Model



