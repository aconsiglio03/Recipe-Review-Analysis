# Recipe-Review-Analysis
Final project for DSC 80, regarding doing meaningful analysis on a dataset containing recipes and the reviews associated with them.


### Introduction
Our datasets consist of 2 csv files. The first is RAW_interactions.csv, which contains a user ID, recipe ID, and then the review and rating a user left. The other is RAW_recipes.csv, which gives us information about the recipes themselves. This includes the tags on the recipe, number of steps, nutrition facts, etc. The one question our project centers around is "Do unhealthier recipes have higher ratings and can we predict the rating a user gives a recipe based on the variables in our dataframe?" This question is interesting because it gives some insight into why people enjoy certain recipes more than others. Is it simply that certain recipes just taste better, or could it be the work that goes into making a meal and whether people naturally enjoy unhealthier recipes? By creating a model and identifying the most significant variables, we can see what types of recipes people naturally gravitate towards.<br><br><br><br>

### Cleaning Process
First, we began by loading in the 2 datasets and following the processes outlined in the write-up. We merged recipes and reviews by keeping recipes and merging on the recipe ID. We filled in ratings of 0 with np.nan because ... We then converted the date reviews were left to datetime to make it easier if we wanted to utilize this column later in the project. We then converted our discrete variables to integers, like number of minutes, number of steps, etc. The most modification we did was extracting the daily value nutritional facts for each macronutrient. We extracted the saturated fat, calories, sugar, etc. to become their own columns and converted their values to integers.

#### DataFrame Head

| name                                 |     id |   minutes |   contributor_id | submitted           | tags                                                                                                                                                                                                                        | nutrition                                                        |   n_steps | steps                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                              | description                                                                                                                                                                                                                                                                                                                                                                       | ingredients                                                                                                                                                                    |   n_ingredients |          user_id |   recipe_id | date                |   rating | review                                                                                                                                                                                                                                                                                                                                           |   avg_rating |   calories |   total_fat_dv |   sugar_dv |   sodium_dv |   protein_dv |   sat_fat_dv |   carb_dv |
|:-------------------------------------|-------:|----------:|-----------------:|:--------------------|:----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|:-----------------------------------------------------------------|----------:|:-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|:----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|:-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|----------------:|-----------------:|------------:|:--------------------|---------:|:-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|-------------:|-----------:|---------------:|-----------:|------------:|-------------:|-------------:|----------:|
| 1 brownies in the world    best ever | 333281 |        40 |           985201 | 2008-10-27 00:00:00 | ['60-minutes-or-less', 'time-to-make', 'course', 'main-ingredient', 'preparation', 'for-large-groups', 'desserts', 'lunch', 'snacks', 'cookies-and-brownies', 'chocolate', 'bar-cookies', 'brownies', 'number-of-servings'] | ['138.4', ' 10.0', ' 50.0', ' 3.0', ' 3.0', ' 19.0', ' 6.0']     |        10 | ['heat the oven to 350f and arrange the rack in the middle', 'line an 8-by-8-inch glass baking dish with aluminum foil', 'combine chocolate and butter in a medium saucepan and cook over medium-low heat , stirring frequently , until evenly melted', 'remove from heat and let cool to room temperature', 'combine eggs , sugar , cocoa powder , vanilla extract , espresso , and salt in a large bowl and briefly stir until just evenly incorporated', 'add cooled chocolate and mix until uniform in color', 'add flour and stir until just incorporated', 'transfer batter to the prepared baking dish', 'bake until a tester inserted in the center of the brownies comes out clean , about 25 to 30 minutes', 'remove from the oven and cool completely before cutting']                                                  | these are the most; chocolatey, moist, rich, dense, fudgy, delicious brownies that you'll ever make.....sereiously! there's no doubt that these will be your fav brownies ever for you can add things to them or make them plain.....either way they're pure heaven!                                                                                                              | ['bittersweet chocolate', 'unsalted butter', 'eggs', 'granulated sugar', 'unsweetened cocoa powder', 'vanilla extract', 'brewed espresso', 'kosher salt', 'all-purpose flour'] |               9 | 386585           |      333281 | 2008-11-19 00:00:00 |        4 | These were pretty good, but took forever to bake.  I would send it ended up being almost an hour!  Even then, the brownies stuck to the foil, and were on the overly moist side and not easy to cut.  They did taste quite rich, though!  Made for My 3 Chefs.                                                                                   |            4 |      138.4 |             10 |         50 |           3 |            3 |           19 |         6 |
| 1 in canada chocolate chip cookies   | 453467 |        45 |          1848091 | 2011-04-11 00:00:00 | ['60-minutes-or-less', 'time-to-make', 'cuisine', 'preparation', 'north-american', 'for-large-groups', 'canadian', 'british-columbian', 'number-of-servings']                                                               | ['595.1', ' 46.0', ' 211.0', ' 22.0', ' 13.0', ' 51.0', ' 26.0'] |        12 | ['pre-heat oven the 350 degrees f', 'in a mixing bowl , sift together the flours and baking powder', 'set aside', 'in another mixing bowl , blend together the sugars , margarine , and salt until light and fluffy', 'add the eggs , water , and vanilla to the margarine / sugar mixture and mix together until well combined', 'add in the flour mixture to the wet ingredients and blend until combined', 'scrape down the sides of the bowl and add the chocolate chips', 'mix until combined', 'scrape down the sides to the bowl again', 'using an ice cream scoop , scoop evenly rounded balls of dough and place of cookie sheet about 1 - 2 inches apart to allow for spreading during baking', 'bake for 10 - 15 minutes or until golden brown on the outside and soft & chewy in the center', 'serve hot and enjoy !'] | this is the recipe that we use at my school cafeteria for chocolate chip cookies. they must be the best chocolate chip cookies i have ever had! if you don't have margarine or don't like it, then just use butter (softened) instead.                                                                                                                                            | ['white sugar', 'brown sugar', 'salt', 'margarine', 'eggs', 'vanilla', 'water', 'all-purpose flour', 'whole wheat flour', 'baking soda', 'chocolate chips']                    |              11 | 424680           |      453467 | 2012-01-26 00:00:00 |        5 | Originally I was gonna cut the recipe in half (just the 2 of us here), but then we had a park-wide yard sale, & I made the whole batch & used them as enticements for potential buyers ~ what the hey, a free cookie as delicious as these are, definitely works its magic! Will be making these again, for sure! Thanks for posting the recipe! |            5 |      595.1 |             46 |        211 |          22 |           13 |           51 |        26 |
| 412 broccoli casserole               | 306168 |        40 |            50969 | 2008-05-30 00:00:00 | ['60-minutes-or-less', 'time-to-make', 'course', 'main-ingredient', 'preparation', 'side-dishes', 'vegetables', 'easy', 'beginner-cook', 'broccoli']                                                                        | ['194.8', ' 20.0', ' 6.0', ' 32.0', ' 22.0', ' 36.0', ' 3.0']    |         6 | ['preheat oven to 350 degrees', 'spray a 2 quart baking dish with cooking spray , set aside', 'in a large bowl mix together broccoli , soup , one cup of cheese , garlic powder , pepper , salt , milk , 1 cup of french onions , and soy sauce', 'pour into baking dish , sprinkle remaining cheese over top', 'bake for 25 minutes or until cheese is lightly browned', 'sprinkle with rest of french fried onions and bake until onions are browned and cheese is bubbly , about 10 more minutes']                                                                                                                                                                                                                                                                                                                              | since there are already 411 recipes for broccoli casserole posted to "zaar" ,i decided to call this one  #412 broccoli casserole.i don't think there are any like this one in the database. i based this one on the famous "green bean casserole" from campbell's soup. but i think mine is better since i don't like cream of mushroom soup.submitted to "zaar" on may 28th,2008 | ['frozen broccoli cuts', 'cream of chicken soup', 'sharp cheddar cheese', 'garlic powder', 'ground black pepper', 'salt', 'milk', 'soy sauce', 'french-fried onions']          |               9 |  29782           |      306168 | 2008-12-31 00:00:00 |        5 | This was one of the best broccoli casseroles that I have ever made.  I made my own chicken soup for this recipe. I was a bit worried about the tsp of soy sauce but it gave the casserole the best flavor. YUM!                                                                                                                                  |            5 |      194.8 |             20 |          6 |          32 |           22 |           36 |         3 |
|                                      |        |           |                  |                     |                                                                                                                                                                                                                             |                                                                  |           |                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                    |                                                                                                                                                                                                                                                                                                                                                                                   |                                                                                                                                                                                |                 |                  |             |                     |          | The photos you took (shapeweaver) inspired me to make this recipe and it actually does look just like them when it comes out of the oven.                                                                                                                                                                                                        |              |            |                |            |             |              |              |           |
|                                      |        |           |                  |                     |                                                                                                                                                                                                                             |                                                                  |           |                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                    |                                                                                                                                                                                                                                                                                                                                                                                   |                                                                                                                                                                                |                 |                  |             |                     |          | Thanks so much for sharing your recipe shapeweaver. It was wonderful!  Going into my family's favorite Zaar cookbook :)                                                                                                                                                                                                                          |              |            |                |            |             |              |              |           |
| 412 broccoli casserole               | 306168 |        40 |            50969 | 2008-05-30 00:00:00 | ['60-minutes-or-less', 'time-to-make', 'course', 'main-ingredient', 'preparation', 'side-dishes', 'vegetables', 'easy', 'beginner-cook', 'broccoli']                                                                        | ['194.8', ' 20.0', ' 6.0', ' 32.0', ' 22.0', ' 36.0', ' 3.0']    |         6 | ['preheat oven to 350 degrees', 'spray a 2 quart baking dish with cooking spray , set aside', 'in a large bowl mix together broccoli , soup , one cup of cheese , garlic powder , pepper , salt , milk , 1 cup of french onions , and soy sauce', 'pour into baking dish , sprinkle remaining cheese over top', 'bake for 25 minutes or until cheese is lightly browned', 'sprinkle with rest of french fried onions and bake until onions are browned and cheese is bubbly , about 10 more minutes']                                                                                                                                                                                                                                                                                                                              | since there are already 411 recipes for broccoli casserole posted to "zaar" ,i decided to call this one  #412 broccoli casserole.i don't think there are any like this one in the database. i based this one on the famous "green bean casserole" from campbell's soup. but i think mine is better since i don't like cream of mushroom soup.submitted to "zaar" on may 28th,2008 | ['frozen broccoli cuts', 'cream of chicken soup', 'sharp cheddar cheese', 'garlic powder', 'ground black pepper', 'salt', 'milk', 'soy sauce', 'french-fried onions']          |               9 |      1.19628e+06 |      306168 | 2009-04-13 00:00:00 |        5 | I made this for my son's first birthday party this weekend. Our guests INHALED it! Everyone kept saying how delicious it was. I was I could have gotten to try it.                                                                                                                                                                               |            5 |      194.8 |             20 |          6 |          32 |           22 |           36 |         3 |
| 412 broccoli casserole               | 306168 |        40 |            50969 | 2008-05-30 00:00:00 | ['60-minutes-or-less', 'time-to-make', 'course', 'main-ingredient', 'preparation', 'side-dishes', 'vegetables', 'easy', 'beginner-cook', 'broccoli']                                                                        | ['194.8', ' 20.0', ' 6.0', ' 32.0', ' 22.0', ' 36.0', ' 3.0']    |         6 | ['preheat oven to 350 degrees', 'spray a 2 quart baking dish with cooking spray , set aside', 'in a large bowl mix together broccoli , soup , one cup of cheese , garlic powder , pepper , salt , milk , 1 cup of french onions , and soy sauce', 'pour into baking dish , sprinkle remaining cheese over top', 'bake for 25 minutes or until cheese is lightly browned', 'sprinkle with rest of french fried onions and bake until onions are browned and cheese is bubbly , about 10 more minutes']                                                                                                                                                                                                                                                                                                                              | since there are already 411 recipes for broccoli casserole posted to "zaar" ,i decided to call this one  #412 broccoli casserole.i don't think there are any like this one in the database. i based this one on the famous "green bean casserole" from campbell's soup. but i think mine is better since i don't like cream of mushroom soup.submitted to "zaar" on may 28th,2008 | ['frozen broccoli cuts', 'cream of chicken soup', 'sharp cheddar cheese', 'garlic powder', 'ground black pepper', 'salt', 'milk', 'soy sauce', 'french-fried onions']          |               9 | 768828           |      306168 | 2013-08-02 00:00:00 |        5 | Loved this.  Be sure to completely thaw the broccoli.  I didn&#039;t and it didn&#039;t get done in time specified.  Just cooked it a little longer though and it was perfect.  Thanks Chef.                                                                                                                                                     |            5 |      194.8 |             20 |          6 |          32 |           22 |           36 |         3 |
<br><br><br><br>

#### Aggregating

|   ('minutes', 'count') |   ('minutes', 'mean') |   ('minutes', 'std') |   ('minutes', 'min') |   ('minutes', '25%') |   ('minutes', '50%') |   ('minutes', '75%') |   ('minutes', 'max') |   ('sat_fat_dv', 'count') |   ('sat_fat_dv', 'mean') |   ('sat_fat_dv', 'std') |   ('sat_fat_dv', 'min') |   ('sat_fat_dv', '25%') |   ('sat_fat_dv', '50%') |   ('sat_fat_dv', '75%') |   ('sat_fat_dv', 'max') |   ('calories', 'count') |   ('calories', 'mean') |   ('calories', 'std') |   ('calories', 'min') |   ('calories', '25%') |   ('calories', '50%') |   ('calories', '75%') |   ('calories', 'max') |   ('sugar_dv', 'count') |   ('sugar_dv', 'mean') |   ('sugar_dv', 'std') |   ('sugar_dv', 'min') |   ('sugar_dv', '25%') |   ('sugar_dv', '50%') |   ('sugar_dv', '75%') |   ('sugar_dv', 'max') |   ('n_steps', 'count') |   ('n_steps', 'mean') |   ('n_steps', 'std') |   ('n_steps', 'min') |   ('n_steps', '25%') |   ('n_steps', '50%') |   ('n_steps', '75%') |   ('n_steps', 'max') |   ('n_ingredients', 'count') |   ('n_ingredients', 'mean') |   ('n_ingredients', 'std') |   ('n_ingredients', 'min') |   ('n_ingredients', '25%') |   ('n_ingredients', '50%') |   ('n_ingredients', '75%') |   ('n_ingredients', 'max') |
|-----------------------:|----------------------:|---------------------:|---------------------:|---------------------:|---------------------:|---------------------:|---------------------:|--------------------------:|-------------------------:|------------------------:|------------------------:|------------------------:|------------------------:|------------------------:|------------------------:|------------------------:|-----------------------:|----------------------:|----------------------:|----------------------:|----------------------:|----------------------:|----------------------:|------------------------:|-----------------------:|----------------------:|----------------------:|----------------------:|----------------------:|----------------------:|----------------------:|-----------------------:|----------------------:|---------------------:|---------------------:|---------------------:|---------------------:|---------------------:|---------------------:|-----------------------------:|----------------------------:|---------------------------:|---------------------------:|---------------------------:|---------------------------:|---------------------------:|---------------------------:|
|                   2821 |               71.9724 |              98.2748 |                    1 |                   20 |                   40 |                   70 |                  570 |                      2821 |                  42.5292 |                 66.2046 |                       0 |                       7 |                      22 |                      53 |                    1013 |                    2821 |                449.812 |               515.133 |                     0 |                170.4  |                 313   |                528.9  |                4887.9 |                    2821 |                76.6813 |               163.489 |                     0 |                     9 |                    27 |                    80 |                  4001 |                   2821 |              10.5587  |              6.7224  |                    1 |                    6 |                    9 |                   13 |                   55 |                         2821 |                     8.90323 |                    3.67368 |                          2 |                          6 |                          8 |                         11 |                         30 |
|                   2323 |               68.1885 |              89.1063 |                    1 |                   25 |                   40 |                   70 |                  550 |                      2323 |                  41.6707 |                 60.995  |                       0 |                       7 |                      24 |                      54 |                     987 |                    2323 |                431.719 |               430.446 |                     0 |                179.75 |                 325.6 |                522.95 |                4674.2 |                    2323 |                67.9875 |               144.1   |                     0 |                     9 |                    25 |                    68 |                  1973 |                   2323 |              10.5902  |              6.42172 |                    1 |                    6 |                    9 |                   13 |                   61 |                         2323 |                     9.21093 |                    3.61084 |                          1 |                          7 |                          9 |                         11 |                         27 |
|                   7081 |               65.0967 |              90.227  |                    1 |                   20 |                   40 |                   60 |                  570 |                      7081 |                  37.5448 |                 53.486  |                       0 |                       7 |                      21 |                      48 |                     789 |                    7081 |                406.202 |               401.216 |                     0 |                173.1  |                 308.5 |                500.3  |                4605.8 |                    7081 |                59.0606 |               132.515 |                     0 |                     9 |                    23 |                    58 |                  2699 |                   7081 |               9.92656 |              6.20069 |                    1 |                    6 |                    9 |                   12 |                   88 |                         7081 |                     9.18331 |                    3.65295 |                          1 |                          7 |                          9 |                         11 |                         27 |
|                  36919 |               58.7495 |              82.8835 |                    1 |                   20 |                   35 |                   60 |                  590 |                     36919 |                  35.2706 |                 50.4365 |                       0 |                       6 |                      20 |                      46 |                     996 |                   36919 |                392.858 |               392.388 |                     0 |                171.9  |                 301   |                481.05 |                4907.1 |                   36919 |                52.824  |               120.528 |                     0 |                     8 |                    21 |                    52 |                  3733 |                  36919 |               9.53563 |              5.65677 |                    1 |                    6 |                    8 |                   12 |                   81 |                        36919 |                     9.09291 |                    3.65626 |                          1 |                          6 |                          9 |                         11 |                         30 |
|                 167425 |               55.9185 |              76.4065 |                    0 |                   20 |                   35 |                   60 |                  585 |                    167425 |                  37.4513 |                 53.1396 |                       0 |                       7 |                      22 |                      49 |                    1166 |                  167425 |                396.793 |               408.616 |                     0 |                168.8  |                 296.8 |                482.9  |                4967.6 |                  167425 |                57.7572 |               129.027 |                     0 |                     8 |                    22 |                    57 |                  4005 |                 167425 |               9.9276  |              6.32797 |                    1 |                    6 |                    9 |                   13 |                   87 |                       167425 |                     9.04122 |                    3.84306 |                          1 |                          6 |                          9 |                         11 |                         33 |

<br>
This grouped table shows us how each quantitative variable breaks down over each rating category. This table is incredibly helpful because it helps us identify which variables might be significant when it comes to determining a recipes rating.
<br><br><br>

### Univariate Analysis
![ratings](https://github.com/user-attachments/assets/39f2a355-2004-41f2-a6aa-2e53fb402f7e)

<br><br>
Here is a histogram detailing the distributions of ratings in our dataframe. As you can see, people disproportionately leave 5 star reviews. 4 stars is the next most common rating, although much less frequent than 5 star reviews. This shows that, at least in our dataset, people only leave reviews when they feel strongly positive about a recipe.

<br><br><br><br>

### Bivariate Analysis
![sat](https://github.com/user-attachments/assets/dab4da0d-9254-470e-af99-bbcb4630db98)
![minutes](https://github.com/user-attachments/assets/c93969cb-fc70-47ad-b88f-d98d17f0dab5)



<br>
Here we attempted to establish a relationship between the healthiness and the minutes required for our recipes and the ratings associated with them. We initially believed that people would rate recipes with higher Saturated Fat contents higher because people generally enjoy the tastes of unhealthier foods. However, this boxplot shows little difference between the average Saturated Fat in each rating category. The same goes for minutes, although it does seem like higher ratings are usually from slightly faster recipes (35 minute medians for 4 and 5 stars, 40 minutes for all other categories).


<br><br><br><br>


### Assessment of Missingness

One column we know to be NMAR is 'Rating'. This is because we were advised to replace ratings of 0 starts with np.nan during our cleaning process because... Therefore, 'Rating' is missing dependent on the value of the rating itself, making it NMAR.

![desc](https://github.com/user-attachments/assets/0bf2470f-7aa2-45ef-83a7-453779f37c86)


![descnot](https://github.com/user-attachments/assets/e60ed7b5-2892-4b9d-b84e-3bf6bf2c1836)


<br><br>
To check if 'Description' was MAR on 'Rating' we performed a permutation test that iterated 3000 times. We got a significantly low p value when using the absolute difference of means of 0.013. We see this significant difference in the histograms of 'Rating' when 'Description' is missing or not missing. We found 'Description' to not be MAR on 'Number of Steps', i.e., 'Number of Steps' had no effect on the missingness of the 'Description' column. For that permutation test we got a p value of 0.21.


<br>

### Hypothesis Testing

$H_{0}$: All recipe ratings regardless of the recipe's tags are part of the same rating distribution.\
$H_{1}$: Recipes marked easy (have the tag '3-steps-or-less', 'easy', or 'beginner-cook') have a higher rating distribution.
<br>
We created an easy df with an extra column determining whether or not these tags exist in a recipe's tags. 0.58 recipes were classified as 'easy', a good divide to test on. We then permuted the 'easy' column 5000 times, and observed whether the means were more extreme than 0.01, the difference in mean ratings. Through our test, we got a p-value of 0.0004, so we reject our null hypothesis and instead hypothesize that recipes with tags referring to their easiness have a higher distribution of ratings.

<br>
We also did a hypothesis test on Low vs High sugar recipes (cutoff at 20% of daily value).

$H_{0}$: All recipe ratings regardless of the sugar are part of the same rating distribution.\
$H_{1}$: Recipes with lower sugar (<20% of daily value) have a higher rating distribution.
<br><br>
We created a sugar df with a column for low sugar (marked by our cutoff) and performed  5000 iterations of our hypothesis test, and found a p value of 0.0002. This is a very significant value, and allows us to reject the null hypothesis. This suggests that recipes with sugar DV <20% have a higher rating distribution in our dataset.

![lowsug](https://github.com/user-attachments/assets/706007a5-9538-472a-af6d-531e7123c511)
![highsug](https://github.com/user-attachments/assets/92459cd0-8802-4182-bfc8-cd869bec9af8)



### Framing a Prediction Problem

For our model we wanted to predict the 'rating' column of our data because we thought it was the most interesting variable. It tells you very simply and clearly whether or not people enjoyed the recipe and if they would recommend it to others. Because we need to build a model that takes into consideration the data we would have at the time, we only utilized variables given in the original recipe, nothing from a user's review. If we're able to create a successful model that uses attributes of a recipe, we're able to get some insight as to what makes a good recipe, and why people may be drawn to specific kinds of recipes. Since we're only using information from the recipes, we grouped each unique recipe and created a column 'avg_rating' which states the mean rating of each recipe. We then rounded the means to the nearest whole number, because we believed we'd have better luck creating a **classifier** model rather than a regression model. Because ratings have 5 'classes' (1-5 stars), this is a **multiclass** classification.

<br><br>

### Baseline Model


<br><br>

### Final Model


<br><br>

### Fairness Model

Because our model regards recipes and ratings, it wasn't immediately apparent what groups we would analyze to ensure fairness. Thinking more in depth, we realized that we should take into account whether our model is assessing certain ethnic foods differently than the rest of the data. We took a few subsets of data for recipes that contained "chinese", "african", "indian", etc. to test our model on.
