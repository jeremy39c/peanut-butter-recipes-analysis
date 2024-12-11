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

PLOT FOR 1 COLUMN, 1-2 sent. explanation w/ description/interpretation
of trends

<iframe
  src="assets/has_pb_bar.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>



### Bivariate Analysis

PLOT FOR TWO COLUMNS, 1-2 sent. explanation w/ description/interpretation
of trends

<iframe
  src="assets/ratings_by_pb_hist.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>

<iframe
  src="assets/sat_fat_by_pb_hist.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>

### Interesting Aggregates

GROUPED/PIVOT TABLE, explanation of significance



## Assessment of Missingness

### NMAR Analysis

STATEMENT ON A COLUMN IN DATA THAT IS NMAR, explanation of reasoning +
additional data to obtain that could explain missingness; USE "NMAR"

### Missingness Dependency

PRESENT, INTERPRET MISSINGNESS PERMUTATION TESTS' RESULTS WITH RESPECT TO 
DATA, QUESTION; EMBED PLOT RELATED TO MISSINGNESS EXPLORATION (distribution of
one column when another is missing/not missing, empirical distribution of test 
statistic used in permutation tests + observed statistic)



## Hypothesis Testing

STATE HYPOTHESES, TEST STATISTIC, SIGNIFICANCE LEVEL, P-VALUE, CONCLUSION;
justify why choices are good for answering question



## Framing a Prediction Problem

STATE PREDICTION PROBLEM AND TYPE; STATE WHETHER BINARY OR MULTICLASS 
CLASSIFICATION; REPORT RESPONSE VARIABLE (PREDICTORY), WHY YOU CHOSE IT, 
METRIC TO EVALUATE MODEL AND WHY CHOSEN OVER OTHERS; JUSTIFY INFORMATION AT 
"TIME OF PREDICTION" AND ONLY USE THOSE FEATURES FOR TRAINING



## Baseline Model

DESCRIBE MODEL, STATE FEATURES, HOW MANY ARE QUANTITATIVE/ORDINAL/NOMINAL, 
HOW ENCODINGS PERFORMED; REPORT MODEL PERFORMANCE, WHETHER OR NOT MODEL IS
GOOD AND WHY



## Final Model

STATE FEATURES ADDED AND WHY THEY ARE GOOD FOR DATA AND PREDICTION TASK (CAN'T
JUST SAY THEY IMPROVED ACCURACY, INSTEAD TALK ABOUT WHY FEATURES IMPROVED
MODEL'S PERFORMANCE); DESCRIBE CHOSEN MODELING ALGORITHM, HYPERPARAMETERS THAT
PERFORMED BEST, METHOD USED TO SELECT HYPERPARAMETERS, OVERALL MODEL; DESCRIBE
HOW FINAL PERFORMANCE IS IMPROVEMENT OVER BASELINE MODEL



## Fairness Analysis

CLEARLY STATE CHOICE OF X AND Y, EVAL METRIC, HYPOTHESES, TEST STATISTIC, 
SIGNIFICANCE LEVEL, RESULTING P-VALUE, CONCLUSION