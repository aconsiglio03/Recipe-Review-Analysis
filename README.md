# Recipe-Review-Analysis
Final project for DSC 80, regarding doing meaningful analysis on a dataset containing recipes and the reviews associated with them.

Our datasets consist of 2 csv files. The first is RAW_interactions.csv, which contains a user ID, recipe ID, and then the review and rating a user left. The other is RAW_recipes.csv, which gives us information about the recipes themselves. This includes the tags on the recipe, number of steps, nutrition facts, etc. The one question our project centers around is "Do unhealthier recipes have higher ratings and can we predict the rating a user gives a recipe based on the variables in our dataframe?" This question is interesting because it gives some insight into why people enjoy certain recipes more than others. Is it simply that certain recipes just taste better, or could it be the work that goes into making a meal and whether people naturally enjoy unhealthier recipes? By creating a model and identifying the most significant variables, we can see what types of recipes people naturally gravitate towards.\



![ratingsdist](https://github.com/user-attachments/assets/77387250-4ddf-434a-aa7c-3eb7f9eefed6)
\
Here is a histogram detailing the distributions of ratings in our dataframe. As you can see, people disproportionately leave 5 star reviews. 0 and 4 stars are the next most common ratings, although much less frequent than 5 star reviews. This shows that, at least in our dataset, people only leave reviews when they feel strongly about a recipe, and in general when they feel strongly positive.
