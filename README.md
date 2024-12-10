# Peanut Butter Recipes Analysis
### Jeremy Chun

Repository for a comprehensive project completed as part of UCSD's DSC 80 
course. An analysis of a recipes dataset, with a focus on the use of 
peanut butter as an ingredient.
'''

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
many for both its palatable and nutrional traits, can help determine the 
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


### Univariate Analysis


### Bivariate Analysis


### Interesting Aggregates


## Assessment of Missingness

### NMAR Analysis


### Missingness Dependency


## Hypothesis Testing


## Framing a Prediction Problem


## Baseline Model


## Final Model


## Fairness Analysis