# Peanut Butter Recipes Analysis
### Jeremy Chun

## Introduction

The dataset of interest in this project, taken from 
[food.com](https://www.food.com), contains information on recipes, such as 
their ingredients, as well as their reviews, including the ratings given.

The focus question driving the following analysis can best be summarized as, 
"Are recipes containing peanut butter distinguishable from other recipes?".

This project seeks to examine any trends in how online recipes are constructed 
and rated, in doing so attempting to establish a distinction between recipes 
predicated on their inclusion or exclusion of peanut butter. The resulting 
conclusions serve to offer insight on how peanut butter, a product favored by 
many for both its palatable and nutrional traits, can help deiermine the 
quality of recipes.

Below one can find relevant information about the dataset, including its 
rows and columns:

Number of Rows: 234,429

Columns:
 - name (Recipe name)
 - recipe_id (Recipe ID)
 - minutes (Minutes to prepare recipe)
 - contributor_id (User ID who submitted recipe)
 - submitted (Date recipe was submitted)
 - tags (Food.com tags for recipe)
 - nutrition (Nutrition information in the form [calories (#), total fat (PDV), 
 sugar (PDV), sodium (PDV), protein (PDV), saturated fat (PDV), carbohydrates 
 (PDV)]; PDV stands for "percentage of daily value")
 - n_steps (Number of steps in recipe)
 - steps (Text for recipe steps, in order)
 - description (User-provided description)
 - ingredients (Recipe ingredients)
 - n_ingredients (Number of ingredients for recipe)
 - user_id (User ID who submitted recipe review)
 - date (Date of interaction)
 - rating (Rating given)
 - avg_rating (Average of ratings given)



## Data Cleaning and Exploratory Data Analysis

### Data Cleaning

The following steps were taken to clean the data in preparation for further 
analysis:

1. Transform String values that look like lists to actual lists.
    - Since the values in the tags, steps, and ingredients columns are Strings 
      that appear to be lists, in which individual values are separated by 
      commas in brackets, it makes sense to map the values to lists containing 
      the relevant comma separated elements. From doing this, it becomes 
      easier to work with the column values for analysis.

2. Split the values in the nutrition column into separate columns for each
   nutrient.
    - As in step 1, the nutrition column consists of Strings that look like 
      lists, so these values were mapped to lists. To go even further, the 
      lists were split element-wise into separate columns so that each nutrient
      (calories, total fat, etc.) is contained in an individual column. 
      Performing this process allows for deeper, more targeted analyses of 
      the nutrients across all recipes.

3. Replace missing data in the description column with NaN values.
    - Some values in the description column were represented as "-------------"
      which can be reasonably equated to missing values. Converting these to 
      NaN values allows for a more accurate representation of missing and 
      non-missing values in the data.

4. Create a boolean column "contains_pb" indicating whether or not a recipe
   contains peanut butter.
    - Taking into consideration the primary focus of the project, a useful 
      column would be one that indicates whether or not a given recipe contains
      peanut butter in its ingredients. Therefore, a boolean column 
      "contains_pb" was constructed by checking if peanut butter was an 
      element in the ingredients column list for each recipe. This will prove 
      key to performing comparative analyses between the two recipe 
      classifications.

5. Filter the data to include only recipes with a calorie count between 50 and 
   2,000 calories.
    - A notable insight drawn from examining the dataset is the presence of 
      unexpected recipes, such as a recipe for "garbage disposal cleaner", as 
      well as recipes with irregular nutritional values, such as a recipe for 
      "powdered hot cocoa mix" with a calorie count of 45,609. Because it does 
      not seem fitting to include these types of recipes in the analysis, one 
      reasonable solution seemed to be filtering out recipes with very little 
      calories, as these are likely to not be conventional food recipes, and 
      setting a limit on how many calories are included, which prevents the 
      inclusion of recipes with absurd nutrient proportions resulting from how 
      the servings were chosen. In carrying out this procedure, the goal is to 
      screen the recipes in a simple, sensible, and effective way to ensure 
      the inclusion of only the appropriate ones for analysis.


Head of the cleaned DataFrame:

|   recipe_id | name                                 |   minutes |   contributor_id | submitted   | tags                                                       |   n_steps | steps                                                 | description                                       | ingredients                                                  |   n_ingredients | ... | rating |   avg_rating |   calories |   total fat |   sugar |   sodium |   protein |   saturated fat |   carbohydrates | contains_pb   |
|------------:|:-------------------------------------|----------:|-----------------:|:------------|:-----------------------------------------------------------|----------:|:------------------------------------------------------|:--------------------------------------------------|:------------------------------------------------------------:|----------------:|----:|-------:|-------------:|-----------:|------------:|--------:|---------:|----------:|----------------:|----------------:|:--------------|
|      333281 | 1 brownies in the world    best ever |        40 |           985201 | 2008-10-27  | ['60-minutes-or-less', 'time-to-make', 'course', 'mai...'] |        10 | ['heat the oven to 350f and arrange the rack in...']  | these are the most; chocolatey, moist, rich, d... | ['bittersweet chocolate', 'unsalted butter', 'eggs...']      |               9 | ... |      4 |            4 |      138.4 |          10 |      50 |        3 |         3 |              19 |               6 | False         |
|      453467 | 1 in canada chocolate chip cookies   |        45 |          1848091 | 2011-04-11  | ['60-minutes-or-less', 'time-to-make', 'cuisine', 'pr...'] |        12 | ['pre-heat oven the 350 degrees f', 'in a mixing...'] | this is the recipe that we use at my school ca... | ['white sugar', 'brown sugar', 'salt', 'margarine', 'eg...'] |              11 | ... |      5 |            5 |      595.1 |          46 |     211 |       22 |        13 |              51 |              26 | False         |
|      306168 | 412 broccoli casserole               |        40 |            50969 | 2008-05-30  | ['60-minutes-or-less', 'time-to-make', 'course', 'mai...'] |         6 | ['preheat oven to 350 degrees', 'spray a 2 quart...'] | since there are already 411 recipes for brocco... | ['frozen broccoli cuts', 'cream of chicken soup...']         |               9 | ... |      5 |            5 |      194.8 |          20 |       6 |       32 |        22 |              36 |               3 | False         |
|      306168 | 412 broccoli casserole               |        40 |            50969 | 2008-05-30  | ['60-minutes-or-less', 'time-to-make', 'course', 'mai...'] |         6 | ['preheat oven to 350 degrees', 'spray a 2 quart...'] | since there are already 411 recipes for brocco... | ['frozen broccoli cuts', 'cream of chicken soup...']         |               9 | ... |      5 |            5 |      194.8 |          20 |       6 |       32 |        22 |              36 |               3 | False         |
|      306168 | 412 broccoli casserole               |        40 |            50969 | 2008-05-30  | ['60-minutes-or-less', 'time-to-make', 'course', 'mai...'] |         6 | ['preheat oven to 350 degrees', 'spray a 2 quart...'] | since there are already 411 recipes for brocco... | ['frozen broccoli cuts', 'cream of chicken soup...']         |               9 | ... |      5 |            5 |      194.8 |          20 |       6 |       32 |        22 |              36 |               3 | False         |

### Univariate Analysis

<iframe
  src="assets/has_pb_bar.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>

The above barplot displays the distribution of recipes based on their inclusion
or exclusion of peanut butter. It is found that the vast majority of recipes 
do not contain peanut butter, with only around 1.06% of the recipes containing 
peanut butter.

### Bivariate Analysis

PLOT FOR TWO COLUMNS, 1-2 sent. explanation w/ description/interpretation
of trends

<iframe
  src="assets/ratings_by_pb_hist.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>

The above histograms display the difference in the distribution of ratings for 
recipes that contain peanut butter and for those that do not. It can be seen 
that while the distributions are relatively similar, where the ratings are 
skewed left and majorly 5's, recipes not containing peanut butter have a 
slightly higher proportion of 5's (~0.77 vs. ~0.73),which could be an 
interesting point of consideration when thinking about how recipes in the two 
groups may be rated differently.

<iframe
  src="assets/sat_fat_by_pb_hist.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>

The above histograms display how saturated fat content is distributed for 
recipes containing and not containing peanut butter. While both plots show a 
right-skewed shape, there appears to be a slight difference in the 
distributions in that the plot for peanut butter recipes is more jagged or 
bumpy and less smooth. It is important to consider that this observation could
stem from the fact that there are many more recipes without peanut butter, 
meaning the smoother distribution could be the result of having more points.

### Interesting Aggregates

| contains_pb   |   avg_rating Mean |
|:--------------|------------------:|
| True          |           4.53571 |
| False         |           4.62482 |

The above table provides the mean of the average ratings for recipes that 
contain and do not contain peanut butter. While it can be seen that non-peanut
butter recipes have a slightly higher average rating mean, it cannot be 
concluded that the difference is significant enough to show that recipes with 
peanut butter are rated lower on average without further testing.

| contains_pb   |   saturated fat Mean |
|:--------------|---------------------:|
| True          |              28.2987 |
| False         |              33.4937 |

The above table provides the mean of the saturated fat content for recipes that
contain and do not contain peanut butter. Again, while it can be observed that
non-peanut butter recipes have a higher mean value, this does not necessarily 
signify a true difference between the groups.



## Assessment of Missingness

### NMAR Analysis

Through examining the data, it was found that the name, description, and rating
columns all contain missing values. However, there does not seem to be any 
indications or intuitive reasonings that can point to any of these missing data
being likely "NMAR". That is, for each of these column values, there is not a 
clear identification of specific groups who would be more likely to not include
information when making a review so that there is a systematic missingness in 
the data.

### Missingness Dependency

One column that can be evaluated in regards to its missingness is the 
description column, which contains the description of the recipe being 
reviewed. To identify whether the data is MCAR or MAR when conditioned on 
other columns of the dataset, one appropriate method would be performing 
permutation tests that consider whether there is a significant difference 
between the distribution of the data when the value in the description column 
is missing and not missing.

In adhering to the above procedure, one permutation test was performed to 
determine the likely missingness of the description column conditioned on the 
minutes column. When using the absolute difference in means of minutes as the 
test statistic and a significance level of 0.05, the p-value was calculated to
be 0.433, meaning there is not evidence to reject the null hypothesis, that 
there is not a difference in the distribution of minutes depending on the 
missingness of description. So, description is most likely MCAR conditioned 
on minutes.

Another permutation test performed examined the likely missingness of the 
description column conditioned on the rating column.
Below are the distributions of rating when the description is missing and not
missing:

<iframe
  src="assets/missing_rating_dist.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>

<iframe
  src="assets/nonmissing_rating_dist.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>

Since the shapes of the distributions are relatively similar, it is appropriate 
to use as the test statistic the absolute difference of means to measure the 
difference between the distributions.

When performing the performing the permutation test at a significance level of 
0.05, the p-value was calculated to be 0.014, meaning there is sufficient 
evidence to reject the null hypothesis and conclude that there is a difference 
in the distribution of rating depending on the missingness of description.
So, description is most likely MAR conditioned on rating.



## Hypothesis Testing

In considering how recipes with peanut butter are different from other recipes, 
a hypothesis test can be performed to compare the distribution of average 
ratings for the two groups. Because it was observed from the EDA that recipes 
with peanut butter had a slightly lower average rating mean value, the 
hypotheses should ideally look at the alternative reality where the average 
ratings of these recipes are really less than other recipes.

The hypotheses used are as follows:

Null Hypothesis: The average ratings of recipes containing peanut butter are 
the same as recipes not containing peanut butter.
Alternative Hypothesis: The average ratings of recipes containing peanut butter 
are less than those for recipes not containing peanut butter.

An appropriate test statistic to be used is the difference in group means 
between non-peanut butter and peanut butter recipes since higher values 
indicate a greater difference in average ratings in favor of non-peanut butter 
recipes. The significance level was set at alpha = 0.05.

From performing a permutation test under the aforementioned specifications, 
the following empirical distribution for the test statistic along with the 
observed test statistic is displayed below:

<iframe
  src="assets/test_stat_dist.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>

Since the calculated p-value is less than 0.05, there is sufficient evidence to 
reject the null hypothesis.
So, there is reason to believe that the average ratings of recipes containing 
peanut butter are indeed lower than those of recipes not containing peanut 
butter.



## Framing a Prediction Problem

In identifying any distinguishing factors between recipes containing peanut 
butter and other recipes, a significant prediction problem is being able to 
predict whether or not a recipe being reviewed contains peanut butter, 
which requires the development of a binary classification model. 

The response variable that will be predicted is the value in the contains_pb 
column, which has the value "True" if the recipe contains peanut butter and 
"False" otherwise. In predicting this classifier, the hope is to establish a 
distinction between the two types of recipes as far as identifying them goes.

The primary metric that will be used to evaluate the model is the F1-score, 
which notably depends on the precision and recall values from the predictions. 
The F1-score is preferable in this instance because indicates a model's ability 
to identify recipes with peanut butter both meaningfully well and often. Other 
metrics, such as accuracy, may not be as suitable since, as seen in the EDA, 
the dataset is majorly imbalanced toward recipes not containing peanut butter, 
which could lead to "good" values that arise from the model constantly making 
the same prediction without actually distinguishing between the groups.

An important consideration to be made for the model training is acknowledging 
the information that would be known at the "time of prediction", which discerns 
between information that can be used to make predictions and information that 
should not be considered for various reasons, such as it not being relevant. 
For this prediction problem, the information at the "time of prediction" 
includes the dataset's columns with the exception of the name, description, and
ingredients columns, which all may explicitly state that a recipe contains 
peanut butter and are additionally undesired as possible features, so these 
columns will not be included as model features.



## Baseline Model

The baseline model serves as a simple model that aims to provide a minimum 
performance level for future, more complex models. This model will likely be 
built upon in future iterations to better understand the nuances of the data.

The baseline model used in this instance is a decision tree classifier with no 
tuned hyperparameters. Importantly, though, the model will account for the 
imbalanced data by balancing the class weights, meaning the minority, recipes 
containing peanut butter, will be "valued" more than the majority, recipes not 
containing peanut butter, in the training and prediction process.
The features used are the rating and saturated fat columns, which are both 
quantitative and not encoded. These features were chosen based on previous 
analyses that suggested potential differences in the distributions between the 
two recipe groups.

After train-test splitting the data and fitting the model on the training data, 
the generated predictions resulted in an F1-score of around 0.03236. 
Based on this result, it is clear that the model is not very good at 
classifying the recipes, likely due to a combination of lacking or insufficient 
features, as well as inadequate performance capabilities that can truly capture 
insights from the data.



## Final Model

Adding onto the baseline model, the selected features include the previous 
features in addition to the protein, total fat, calories, and sodium columns, 
which are all quantitative and not encoded. These features were chosen based on 
an understanding of the macronutrient breakdown of peanut butter and the 
general characteristics of it as an ingredient. Intuitively, the realization 
that peanut butter is a food higher in calories, primarily consisting of 
fat and protein, and often containing considerable amounts of salt, gives way 
to the inference that recipes using it are likely to reflect these qualities. 
This is on top of the fact that recipes not containing peanut butter may have 
alternative ingredients, such as butter, that have differing nutritional 
profiles that are reflected in the overall recipe. For these reasons, the final 
model's features are mainly focused on how peanut butter may specifically 
influence a recipes' nutritional breakdown and contents when predicting.

The chosen modeling algorithm is the ensemble method of random forest, which is 
more complex and effective at making predictions than a single decision tree.
The hyperparameter tuning process of grid search was used to identify the best 
choice for the max depth of the random forest's decision trees out of possible 
values ranging from 5 to 130, with a step of 20 in between. After performing 
grid search, the best performing hyperparameter selection was a max depth of 45.

After creating a random forest classifier of 100 decision trees, each with a 
max depth of 45 and balanced class weights, the model was fit on the same 
training data to generate predictions on the same test data. When evaluating 
this final model, the F1-score was found to be around 0.832, which is a large 
improvement from the baseline model. This result is likely due to the use of 
more features that are relevant and useful, as well as a more complex algorithm.

The results of the final model's performance indicate that the final model is 
able to correclty predict whether or not a recipe contains peanut butter at a 
fairly good rate on the test data. The model's performance still has room for 
improvement, which could be done through the selection of more or better 
features, better hyperparameter selections, or a different modeling algorithm.



## Fairness Analysis

From observing in the EDA that the vast majority of reviews for all recipes had 
a rating value of 5, one interesting fairness analysis is a comparison of the 
final model's performance on recipes with a review rating of 5 and recipes 
with a review rating below 5. Group X is represented by the recipes with a 
rating of 5, while Group Y is represented by the recipes with a rating below 5.
The evaluation metric in use is the classifier's recall, which measures the 
model's ability to identify the recipes containing peanut butter out of all 
recipes. This metric should ideally maximize the amount of true positives, or 
recipes containing peanut butter that are correctly identified, which could be 
seen as particularly valuable in consideration of the imbalanced dataset with 
a small amount of such recipes.

In performing the fairness analysis through permutation testing, the null 
hypothesis is that the classifier's recall is the same for reviews of rating 
below 5 and reviews of rating 5, and any differences are due to random chance. 
The alternative hypothesis is that the classifier's recall is lower for ratings 
below 5 than ratings equal to 5, implying that the classifier performs worse on 
recipes rated below 5.
The test statistic is the difference in recall between the model's predictions 
on reviews of rating 5 and reviews of rating below 5, and the significance 
level is set at alpha = 0.05.

After performing the permutation test, the resulting p-value was found to be 
around 0.0, meaning there is sufficient evidence to reject the null hypothesis.
Therefore, it can be said that the model likely has a lower recall, and 
therefore worse performance, when making predictions for recipes rated below 5 
when compared to predictions for recipes rated 5. This result could come from 
the fact that the data consists of mostly reviews of rating 5, allowing the 
model to have greater exposure to these instances and more opportunities to 
correcty identify if they contain peanut butter. So, a possible future 
consideration to improve the collected data could be trying to get more reviews 
that do not give the recipe a rating of 5, which would provide better 
representation for these types of reviews and recipes.