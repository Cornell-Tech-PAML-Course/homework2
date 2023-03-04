# Practical Applications in Machine Learning 

# Homework 2: Predict Housing Prices Using Regression

The goal of this assignment is to build a regression machine learning pipeline in a web application to use as a tool to analyze the models to gain useful insights about model performance. Note, that the ‘Explore Dataset’ and ‘Preprocess Data’ steps from homework 1 can be re-used for this assignment.

The <b>learning outcomes</b> for this assignment are:
* Build end-to-end regression pipeline using 1) multiple regression, 2) polynomial regression, and 3) ridge, and 4) lasso regression.
* Evaluate regression methods using standard metrics including root mean squared error (RMSE), mean absolute error (MAE), and coefficient of determination (R2).
* Develop a web application that walks users through steps of the regression pipeline and provide tools to analyze multiple methods across multiple metrics. 
* Develop a web application that offers a service to customers by predicting housing prices using regression models. 

# Assignment Deadline:
* Due:  Friday March 18, 2023 at 11:00PM 
* What to turn in: Submit responses on GitHub AutoGrader
* Assignment Type: Individual
* Time Estimate: 18 Hours
* Submit code via GitHub: https://classroom.github.com/a/7LmoWBZw
* Submit Reflection Assessment via Canvas (7 multiple choice questions)

## Assignment Outline
* Setup
* End-to-End Regression Models
* Testing Code with Github Autograder
* Reflection Assessment

# Reading Prerequisites 

* Review the jupyter notebook in Chapter 4 Training Models of “Machine Géron, Aurélien. Hands-on machine learning with Scikit-Learn, Keras, and TensorFlow.” O’Reilly Media, Inc., 2022. (Available on Canvas->Library Reserves). We will build on that example.
* Review [HW2 Web App Design Slides](https://cornell.box.com/s/dgt5gpp6kiu58n6u6jg24y4k6aqcotfp).

# Computing Prerequisites

* [Optional] Revisit the [Preliminaries Github repository](https://github.com/Cornell-Tech-PAML-Course/0-paml-preliminaries) if you have not set up your computing environment. 
* Clone Homework 2 Github using the [Github Autograder Invitation Link](https://classroom.github.com/a/7LmoWBZw).

# End-to-End Regression Models for Housing Prices 

# California Housing Data

This assignment involves testing the end-to-end pipeline in a web application using a California Housing dataset from the textbook: Géron, Aurélien. Hands-on machine learning with Scikit-Learn, Keras, and TensorFlow. O’Reilly Media, Inc., 2022. The dataset was captured from California census data in 1990. We have added additional features to the dataset. The features include:
* longitude - longitudinal coordinate
* latitude - latitudinal coordinate
* housing_median_age - median age of district
* total_rooms - total number of rooms per district
* total_bedrooms - total number of bedrooms per district
* population - total population of district
* households - total number of households per district'
* median_income - median income
* ocean_proximity - distance from the ocean
* median_house_value - median house value
* city - city location of house
* county - county of house
* road - road of the house
* postcode - zip code 

The Github repository contains two datasets in the dataset directory:
* housing - dataset from the textbook and Kaggle 
* housing_paml_hw2 - modified dataset for HW2. Use this dataset for testing HW1.

# Explore and Visualize Data (3 points)

Your goal is to build on the system you developed from homework 1 and additional options to explore datasets.

## Task 1 

Import or restore a dataset and display it in a table. Your goal is to create two columns 1) upload a dataset from a file on your and 2) upload a dataset from a cloud source (specifically for HW2). Then, store the dataset in streamlit session state for later use. This task is repeated for all pages on the web application except Pages A_Explore Dataset and D_Deploy App. On Pages B - D, make sure to restore the dataset from st.session_state to avoid importing multiple times. Feel free to reuse code from HW1.

<b>Task 2</b>: Generate and show visualizations of features in the dataset. Display at most two features from the dataset in a scatterplot, boxplot, line plot, and histogram, depending on the plot. Once a plot has been selected, the visualization should update with the appropriate figure. Use the user_input_features function to collect filters for each figure and update the plot accordingly. Feel free to reuse code from HW1. 

<b>Task 3</b> (Checkpoint 1): Show feature correlations. Write a function compute_correlation() that computes the correlation between two features specified by a user. Recall: Correlation is a quantitative measure of how much two variables/features are correlated ranging from -1 (negatively correlated) to 1 (positively correlated). Feel free to reuse code from HW1. In addition, include summary statements for each pair of features stating the magnitude and direction of correlation. For example, ‘Features median_house_age and median_housing_price are highly positively correlated.’ See the HW2 code for inputs and outputs.

# Preprocess and Prepare Data for Regression (8 points)

Your goal is to build on the system you developed from HW1 and additional options to preprocess data. 

<b>Task 4 (Checkpoint 2)</b>: Summarize missing or invalid data. Use Pandas function isna() to find the missing values in the dataset. Write a function called summarize_missing_data() that computes four statistics:

* Number of categories with missing values.
* Average number of missing values per category. 
* Total number of missing values.
* Categories with most top N missing values where N is a number greater than 1.  

Feel free to reuse code from HW1 and the [‘Predicting Housing Prices’ notebook](https://github.com/Cornell-Tech-PAML-Course/predicting-housing-prices). See the HW2 code for inputs and outputs.

Task 5 and 6: Add two columns to the web application using st.columns() function, one for Task 6 and 7. 

<b>Task 5</b>: Remove irrelevant/useless features. Collect a user's preferences on one or more features to remove using the st.multiselect() function. Add a button to remove irrelevant/useless values using the pd.drop() function. Feel free to reuse code from HW1.

<b>Task 6 (Checkpoint 3)</b>: Remove Nans. Add a button to remove ‘Not a Number’ (Nan) values from the dataset which calls the remove_nans() using the pd.dropna()  function. Write a function called remove_nans() that returns a dataframe with Nans removed.

<b>Task 7</b>: Impute data. Impute the dataset with one of the following options: 1) mean, 2) median, 3) zeros. Collect a user’s preference for one feature to impute imputation method (from previous sentence). Next, show the updated dataset showing the imputed datapoint updates. 

<b>Task 8 (Checkpoint 4)</b>: Remove outliers. Remove outliers from the dataset using the InterQuartile Range (IQR), a commonly used approach for finding and removing outliers.

Display a plot, as done in Task 2 to view a Scatter Plot, Boxplot, Line Plot, or Histogram to inspect features before removing outliers. Add a st.multiselect() menu with the numerical features as options using the code below, given that df is the Pandas dataframe containing the dataset. 
```
numeric_columns = list(df.select_dtypes(include='number').columns)
```

Then, add a button that calls the remove_outliers function which takes a pandas dataframe (df) and feature (string) as input and returns that dataset with outliers removed from the feature. This function computes the 25th (lower bound) and 75th percentile (upper bound), uses the upper and lower bounds to filter the feature, and returns the updated dataframe. Lastly, the function should display the number of outliers removed from the dataset. For example, ‘Outliers for feature median_income are lower than 2543 and higher than 28374.’ See example in the ‘Predicting Housing Prices’ notebook.

Write a function called remove_outliers() using the following function(s) (See the HW2 code for inputs and outputs):
pd.dropna() - function that drops rows or columns with Nan values. Drop Nan values before removing outliers.
np.percentile() - function that computes the q-th percentile of data along the specified axis. Using this function to compute the 25th and 75th percentile.

<b>Task 9 (Checkpoint 5)</b>: Handling Text/Categorical Data. Encode categorical features with integer encoding and one-hot encoding and write one function for each encoding technique. Add a button for each method which calls the functions below for their respective functions.

Write a function called integer_encode_feature() using pd.get_dummies() function - converts categorical variables to indicator function (i.e., one-hot encoding). Then, use st.button() to create a button to run the function. See the HW2 code for inputs and outputs.

Write a function called one_hot_encode_feature() using OrdinalEncoder fit_transform() - maps categorical variables to unique integers (i.e., integer encoding). Then, use st.button() to create a button to run the function. See the HW2 code for inputs and outputs.

<b>Task 10 (Checkpoint 6)</b>: Create new features. Create new features using existing ones in the dataset using four mathematical operations including 1) addition, 2) subtraction, 3) multiplication, 4) division, and 5) square root, 6) ceil, and 7) floor. Collect two numerical features for the math operations using st.multiselect(), except square root which takes one feature as input using st.selectbox(). See example for collecting numerical columns below:  
```
numeric_columns = list(df.select_dtypes(include='number').columns)
```
Write the create_feature() function that creates a new feature using the output. It takes a pandas dataframe, a list of mathematical operations to apply to one or more features, and a list of one (for square root) or two features (all other features). For example, a user can select to ‘add’ total_bedrooms and total_rooms then the function adds these two features, and names the feature with the feature name specified as input. See the HW2 code for inputs and outputs.

Use the following function(s):
* st.text_input() - function that displays a text field and a user to input text. Collect user input to name the new feature.
* st.button() - function that creates a button. Create a button to create the new feature.
* st.selectbox() - function that creates a menu for selecting a single item. Create a menu with the integer and one-hot encoding.
* st.multiselect() - function that creates a menu for selecting multiple items. Create a menu with features from the dataset as options.
* np.sqrt() - function that computes the square root of a number. Compute the square root of a feature when selected by the user.
* np.ceil() - rounds float values up
* np.floor() - rounds float values down 

<b>Task 11</b>: Summarize descriptive statistics. Feel free to reuse code from HW1.  

# Train Regression Models (7 points)

Set a fixed seed of 10 for all regression models (see example below).
```
import random 
random.seed(10)
```

<b>Task 12</b>: Set train/test split using your code from HW1. In addition, write a function called split_dataset() that splits the dataset into four parts, input and output training and test sets (e.g., X_train, X_val, y_train, y_val)using the following function(s):
number_input(): function that displays a text field and a user to input a number. Collect the test set size from the text field.
Sklearn train_test_split() - function that splits that dataset into training and testing sets. Split the dataset into input X and output Y values for a training and test set.

<b>Task 13</b>: Select model input and output variables, and model parameters. Recall: Regression aims to predict output variable Y from input variable(s) X. Your goal is to set the input and output variables with input from a user using pandas filtering (see example below). The input variable can contain one or more features and the output variable contains one feature to predict (i.e., house prices). 
```
A = df.loc[:, df.columns.isin(feature)] # collects columns with the feature name
B = df.loc[:, ~df.columns.isin(feature)] # collects columns without the feature name
```

Use the following functions:
* st.multiselect() - function that creates a menu for selecting multiple items. Create a menu with features from the dataset as options for regression output.
* st.selectbox() - function that creates a menu for selecting a single item. Create a menu of regression models to train with options: multiple regression, polynomial regression, and ridge regression, and lasso regression.

Use the following functions for Task 14-17:
st.multiselect() - function that creates a menu for selecting multiple items. Create a menu with the four regression models as options.
* st.selectbox() - function that creates a menu for selecting a single item. Create a menu with the four regression models as options. 
* st.text_input() - function that displays a text field and a user to input text. Collect user input for integer parameters (e.g., polynomial degree).
* st.number_input() - function that displays a text field and a user to input a number. Collect user input for integer parameters (e.g., polynomial degree).
* st.button() - function that creates a button. Create a button to start training the models.

<b>Task 14</b>: Write a function called train_multiple_regression() that trains a multiple regression regression model to predict an output variable (i.e., housing prices). Add a button to call the train_multiple_regression() function and train the model using Sklearn library: linear_model.LinearRegression(). See the HW2 code for inputs and outputs.

<b>Task 15</b>: Write a function called train_polynomial_regression() that trains a polynomial regression regression model to predict an output variable (i.e., housing prices). Then, display the training performance in terms of the metrics selected by users. Create a parameter input panel for each method that requires model parameters including degrees and include_bias.  Then, add a button to call the train_polynomial_regression() function and train the model with Sklearn library: linear_model, preprocessing.PolynomialFeatures or Pipeline. See the HW2 code for inputs and outputs.

Use the following functions to collect input from the web app:
* st.number_input to select number of degrees
* st.checkbox to include bias
* st.button that calls function to train polynomial regression models

<b>Task 16</b>: Write a function called train_ridge_regression that uses cross validation to train a ridge regression regression model to predict housing prices and display the training performance in terms of the metrics selected by users. Create a parameter input panel for each method that requires model parameters alpha, solver, and n_folds.  Then, add a button to call the train_ridge_regression function and train the model using Sklearn library: linear_model.Ridge() or Pipeline. See the HW2 code for inputs and outputs.

Use the following functions to collect input from the web app:
* st.number_input to select number of folds
* st.multiselect solver methods including: 'auto', 'svd', 'cholesky', 'lsqr', 'sparse_cg', 'sag', 'saga', 'lbfgs'
* st.text_input to select alpha values separated by a comma 
* st.button that calls function to train ridge models

<b>Task 17</b>: Write a function called train_lasso_regression that uses cross validation (CV) to train a lasso regression regression model to predict housing prices and display the training performance in terms of the metrics selected by users. Create a parameter input panel for each method that requires input parameters including tol, alpha, n_folds.  Then, add a button to call the train_lasso_regression function and train the model using Sklearn library: linear_model.Lasso() or Pipeline. See the HW2 code for inputs and outputs.

Use the following functions to collect input from the web app:
* st.number_input to select number of folds - used to enter number of folds for CV
* st.text_input to select tolerance values separated by a comma Enter alpha values for evaluation - used to enter tolerance values for evaluation 
* st.text_input to select alpha values separated by a comma 
* st.button that calls function to train lasso models - used to train lasso models 

<b>Task 18 (Checkpoint 7)</b>: Inspect Regression coefficients. Write a function called inspect_coefficients() that takes all trained regression models as input and returns their respective coefficients. The Sklearn library implements both closed-form and gradient descent solutions to generate regression coefficients. The coefficients can be inspected using the coef_ and intercept_ attributes (see example below). Once one or more regression models have been trained, display the regression coefficients to be used for inspection (use for reflection assessment). Use st.multiselect menu with features as options - used to select features for regression input.

# Test Regression Models (4 points)

<b>Task 19</b>: Provide users with the option to select up to three metrics for evaluation and with the option to select multiple trained models:

* Root mean squared error (RMSE) measures the difference between predicted and actual values using Euclidean distance. Use Sklearn mean_squared_error() function that computes mean squared error (squared=True) and root mean squared error (squared=False).
* Mean absolute error (MAE) measures the absolute difference between predicted and actual values. Use Sklearn mean_absolute_error() function that computes mean absolute error.
* Coefficient of determination (R2) represents proportion of variance in predicted values that can be explained by the input features. Furthermore, it represents a goodness of fit, thereby, how well the model can generalize to new examples. Use Sklearn * r2_score() function that computes the coefficient of determination (R2).

Write a function called calculate_metrics() that takes the predicted and actual housing values as input and returns the metrics for each method using the following: See the HW2 code for inputs and outputs.

Use the following functions to collect input from the web app:
* st.multiselect menu with metrics as options
* st.multiselect menu with trained regression models as options
* st.button that calls calculate_metrics() function to evaluate models

<b>Task 20 (Checkpoint 8)</b>: Write a function plot_learning_curve() that displays figures summarizing learning curves that show predicted housing prices vs the training set size (error vs. training set size). See the HW2 code for inputs and outputs.

Use the following functions in plot_learning_curve()
* st.plotly_chart() - used to display learning curves. 
* Plotly.subplots make_plots() function to create learning curve plots. Use the fig.add_trace() function to plot training and validation curves. 

Use the following functions to collect input from the web app:
* st.multiselect() - function that creates a menu for selecting a trained model to evaluate.
* st.multiselect() - function that creates a menu for selecting a ‘Learning Curve’ and ‘Metric Results’. 

<b>Task 21 (Checkpoint 9)</b>: Display tables summarizing the results in terms of evaluation metrics by regression model for selected metrics.  Write a function compute_eval_metrics() using the st.dataframe() to display results in tables. This function computes the RMSE, MAE, and R2 score for the selected model and metrics from Task 20. Your goal is to populate the metricl_dict with the metric name as a key and the computed results as values. See the HW2 code for inputs and outputs.

<b>Task 23 (Checkpoint 10)</b>: Before deploying the application, train regression models using the following features: number_bedrooms, number_bathrooms, and ocean_proximity. Then, deploy a regression model with best performance out of the four models implemented including multiple regression, polynomial regression, and ridge regression, and lasso regression for a given metric. Provide the user the option to select a regression model based on the evaluation results to use for deployment performance using the st.selectbox() function to create a menu for selecting a single item. 

# Deploy ML Application (3 points)

Create a California Housing Price application that collects a user's housing preferences to use for predicting housing prices. Once a user provides their preferences, deploy a regression model with the best performance given a pre-defined metric from Task 19.

<b>Task 22</b>: Collect users housing preferences to used as features for regression including:

* Number of bedrooms
* Number of bathrooms
* Desired city
* Proximity to water

<b>Task 23 (Checkpoint 11)</b>: Write a function called deploy_model() which tests a regression model with best performance out of all regression models for a given metric. Use the deployment model selected on the ‘Test Model’ page to train the specified regression model and hyperparameters.  The next element shown on the web application is a map of California showing the available house listings.

<b>Task 24</b>: Use the selected regression model to show housing prices along with a description of it including:

* Housing price 
* Number of bedrooms
* Number of bathrooms
* City
* Proximity to the water

$ Testing Code with Github Autograder

Test your homework solution as needed using Github Autograder. Clone your personal private copy of the homework assignment. Once the code is downloaded and stored on your computer in your directory of choice, you can start testing the assignment. To test your code, open a terminal and navigate to the directory of the homework2.py file. Then, type ‘pytest’ ad press enter. The autograder with print feedback to inform you what checkpoint functions are failing. Test homework2.py using this command in your terminal:

```
streamlit run homework2.py
```

# Reflection Assessment

Complete on Canvas.





