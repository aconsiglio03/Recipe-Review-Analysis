# Recipe-Review-Analysis
Final project for DSC 80, regarding doing meaningful analysis on a dataset containing recipes and the reviews associated with them.

Our datasets consist of 2 csv files. The first is RAW_interactions.csv, which contains a user ID, recipe ID, and then the review and rating a user left. The other is RAW_recipes.csv, which gives us information about the recipes themselves. This includes the tags on the recipe, number of steps, nutrition facts, etc. The one question our project centers around is "Do unhealthier recipes have higher ratings and can we predict the rating a user gives a recipe based on the variables in our dataframe?" This question is interesting because it gives some insight into why people enjoy certain recipes more than others. Is it simply that certain recipes just taste better, or could it be the work that goes into making a meal and whether people naturally enjoy unhealthier recipes? By creating a model and identifying the most significant variables, we can see what types of recipes people naturally gravitate towards.<br><br><br><br>

### Univariate Analysis
![ratings](https://github.com/user-attachments/assets/39f2a355-2004-41f2-a6aa-2e53fb402f7e)

<br><br>
Here is a histogram detailing the distributions of ratings in our dataframe. As you can see, people disproportionately leave 5 star reviews. 0 and 4 stars are the next most common ratings, although much less frequent than 5 star reviews. This shows that, at least in our dataset, people only leave reviews when they feel strongly about a recipe, and in general when they feel strongly positive.

<br><br><br><br>

### Bivariate Analysis
![sat](https://github.com/user-attachments/assets/dab4da0d-9254-470e-af99-bbcb4630db98)
![minutes](https://github.com/user-attachments/assets/c93969cb-fc70-47ad-b88f-d98d17f0dab5)



<br>
Here we attempted to establish a relationship between the healthiness and the minutes required for our recipes and the ratings associated with them. We initially believed that people would rate recipes with higher Saturated Fat contents higher because people generally enjoy the tastes of unhealthier foods. However, this boxplot shows little difference between the average Saturated Fat in each rating category. The same goes for minutes, although it does seem like higher ratings are usually from slightly faster recipes (35 minute medians for 4 and 5 stars, 40 minutes for all other categories).
