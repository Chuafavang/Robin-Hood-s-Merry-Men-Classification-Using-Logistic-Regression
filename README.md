# Robin-Hood-s-Merry-Men-Classification-Using-Logistic-Regression

## Overview

This project uses **logistic regression** to predict whether an archer is a member of **Robin Hood's Merry Men** based on characteristics such as shooting accuracy, age, clothing colour, home location, and jail history.

The analysis was conducted in **R** using the `tidyverse`, `tidymodels`, `inspectdf`, `modelr`, and `car` packages. The project covers data cleaning, exploratory data preparation, model development, interaction analysis, backward stepwise regression, prediction, and model evaluation.

## Objective

The main objective is to develop a logistic regression model that predicts the probability of an archer being a member of Robin Hood's Merry Men.

The response variable is:

* **RHBMM** — whether the archer is a member of Robin Hood's Merry Men (`yes`/`no`).

The predictor variables are:

* **Accuracy** — proportion of arrows that hit the target.
* **Age** — youth, middle, or senior.
* **Dress** — black, green, or red.
* **Home** — city or forest.
* **Jail** — whether the archer has a jail record.

## Dataset

The analysis uses the `merry_0.csv` dataset.

The data was split into:

* **Training set:** 2/3 of the observations
* **Testing set:** 1/3 of the observations

A fixed random seed of `1896853` was used to ensure reproducibility.

## Data Preparation

The following data-taming steps were performed:

* Checked for missing values.
* Converted `RHBMM` from numeric values (`0`/`1`) to a factor with `no` and `yes` levels.
* Renamed variables to lowercase.
* Converted categorical variables to factors.
* Converted `jail` from `yes`/`no` to a logical variable.
* Checked categorical variables for unexpected values.
* Checked `accuracy` to ensure values were within the valid range of 0 to 1.

## Modelling Approach

### Initial Logistic Regression

A logistic regression model was initially fitted using all individual predictor variables:

```r
rhbmm ~ accuracy + age + dress + home + jail
```

The categorical variables were represented using dummy variables, with the following reference levels:

* **Dress:** black
* **Age:** middle
* **Home:** city
* **Jail:** FALSE

Because the initial model contained one continuous predictor and four categorical predictors without interactions, it described **36 parallel lines** when represented in terms of log-odds.

### Interaction Analysis

Second-order interaction terms were then considered:

```r
rhbmm ~ .^2
```

The analysis identified important interactions between:

* `accuracy × home`
* `dress × home`

### Backward Stepwise Regression

A backward stepwise procedure was applied to simplify the model.

Interaction terms were removed sequentially based on statistical significance. The final model retained:

* `accuracy`
* `dress`
* `home`
* `jail`
* `accuracy × home`
* `dress × home`

Age was removed from the final model because it did not meet the 95% significance level.

## Final Model

The final logistic regression model can be expressed as:

$$
\begin{aligned}
\hat{r_i} ={}&
-1.23516
+1.21455,accuracy
+0.95494,dressgreen
-0.18067,dressred\
&+1.03535,homeforest
+0.24523,jailTRUE\
&+1.36636(accuracy \times homeforest)\
&+1.59000(dressgreen \times homeforest)\
&-1.05515(dressred \times homeforest)
\end{aligned}
$$

where $\hat{r_i}$ represents the estimated log-odds of being a member of Robin Hood's Merry Men.

The final model contains two significant interaction effects:

* **Accuracy × Home**
* **Dress × Home**

The `accuracy × home` interaction indicates that the relationship between shooting accuracy and Merry Men membership differs depending on whether an archer lives in the city or forest.

The `dress × home` interaction indicates that the relationship between clothing colour and Merry Men membership also depends on home location.

## Model Evaluation

The final model was evaluated using the independent testing dataset.

| Metric      |    Result |
| ----------- | --------: |
| Accuracy    | **0.677** |
| Sensitivity | **0.662** |
| Specificity | **0.696** |
| AUC         | **0.732** |

### Confusion Matrix

The model produced the following classification results on the testing data:

|                | Predicted No | Predicted Yes |
| -------------- | -----------: | ------------: |
| **Actual No**  |         3026 |          1321 |
| **Actual Yes** |         1910 |          3743 |

The model achieved an accuracy of approximately **67.7%**.

The sensitivity of **66.2%** indicates that the model correctly identifies approximately two-thirds of actual Merry Men members.

The specificity of **69.6%** indicates that the model correctly identifies approximately seven in ten non-members.

### ROC and AUC

The ROC curve was used to evaluate the model's ability to discriminate between Merry Men members and non-members.

The model achieved an **AUC of 0.732**, indicating moderate discriminatory ability.

## Example Prediction

The final model was also used to classify a new archer with:

* Accuracy: `112/116`
* Dress: `green`
* Age: `youth`
* Home: `forest`
* Jail: `FALSE`

The model predicted that the archer was a **Merry Man**, with an estimated probability of approximately **0.992**.

## Key Findings

The analysis suggests that:

* Shooting accuracy is an important predictor of Merry Men membership.
* Home location modifies the relationship between accuracy and membership.
* The effect of clothing colour depends on home location.
* Age was not retained in the final model after model selection.
* The final model achieved an AUC of **0.732** on the testing data.
* The model provides moderate predictive discrimination, although its overall classification accuracy of **67.7%** indicates that there is substantial room for improvement.

## Repository Structure

```text
robin-hood-merry-men-classification/
│
├── data/
│   └── merry_0.csv
│
├── Report/
│   └── merry_men.pdf
│
├── SRC/
│   └── merry_men.Rmd
│
└── README.md
```

## Technologies Used

* **R**
* **R Markdown**
* **tidyverse**
* **tidymodels**
* **inspectdf**
* **modelr**
* **car**
* Logistic Regression
* ROC/AUC analysis
* Confusion Matrix
* Backward Stepwise Regression

## Conclusion

This project demonstrates the application of logistic regression to a binary classification problem. After data preparation and interaction analysis, a final model containing accuracy, dress, home, jail, and their relevant interaction terms was developed.

The model achieved an **accuracy of 67.7%** and an **AUC of 0.732** on the testing data, suggesting moderate predictive performance. The analysis also demonstrates how interaction terms can reveal relationships that would not be captured by considering individual predictors alone.
