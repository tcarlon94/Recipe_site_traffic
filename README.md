# Recipe Traffic Prediction

## Overview

This project evaluates which recipes are most likely to generate high website traffic when featured on a recipe platform's homepage. The analysis combines data-quality validation, exploratory analysis, and binary classification to help the content team make more consistent merchandising decisions.

The primary business requirement was to correctly identify at least **80% of recipes that would generate high traffic**. Because missing a popular recipe represents a lost traffic opportunity, recall for the high-traffic class was selected as the primary model metric.

## Business Questions

- Which recipe characteristics are most associated with high traffic?
- Can high-traffic recipes be identified with at least 80% recall?
- Which model offers the best balance between finding popular recipes and limiting false positives?
- How should the content team incorporate model predictions into homepage planning?

## Dataset and Validation

The modeled dataset contained **885 recipes** after data-quality filtering. Available features included:

- Recipe category
- Number of servings
- Calories
- Carbohydrates
- Sugar
- Protein
- High-traffic outcome

Validation and preparation included:

- Converting serving counts into a consistent numeric format.
- Treating the dataset's missing high-traffic labels as the negative class.
- Imputing missing nutritional values with category-level medians.
- Reviewing implausible nutritional values and removing a small number of extreme records.
- Encoding recipe categories for modeling.
- Splitting the data into training and holdout sets before model fitting.

The validation process also identified broad inconsistencies between reported calories and macronutrients. This is an important limitation: the model can support decisions, but the nutritional fields should be audited before production use.

## Exploratory Findings

Recipe category was the strongest observed indicator of high traffic. Nutritional values and serving size provided comparatively little separation between high- and low-traffic recipes.

### Higher-Traffic Categories

- Vegetable
- Potato
- Pork
- One Dish Meal
- Meat

### Lower-Traffic Categories

- Beverages
- Breakfast
- Chicken

For example, 77 of 78 vegetable recipes and 75 of 80 potato recipes were labeled high traffic, while only 5 of 92 beverage recipes received that label. These patterns make category useful for prioritization, although they should be monitored for changes in audience preference.

## Model Comparison

Two classification approaches were evaluated:

- **Logistic regression:** selected as the interpretable baseline because its coefficients and probabilities are easy to communicate and operationalize.
- **Random forest:** used to test whether nonlinear relationships and feature interactions would improve performance.

| Model | Accuracy | High-Traffic Precision | High-Traffic Recall | High-Traffic F1 |
|---|---:|---:|---:|---:|
| Logistic regression | 73% | 76% | **81%** | 79% |
| Random forest | 70% | 74% | 79% | 76% |

Logistic regression was selected because it met the business requirement with **81% recall** and outperformed the random forest across the primary metric, precision, F1 score, and overall accuracy.

The model's 76% precision means that some recipes predicted to generate high traffic will underperform. This is an acceptable tradeoff when the business places a higher cost on overlooking genuinely popular content.

## Business Recommendations

- Use model scores to create a shortlist of homepage candidates rather than automating content selection completely.
- Prioritize historically strong categories while maintaining room for editorial judgment and content variety.
- Track high-traffic recall after deployment to detect performance degradation.
- Record model scores, placement decisions, and realized traffic so that future models can learn from actual merchandising outcomes.
- Collect additional features such as preparation time, ingredient cost, seasonality, user ratings, click-through rate, and prior homepage exposure.
- Validate category strategy through controlled homepage experiments before treating category differences as causal.

## Workflow

1. Validated column types, missingness, duplicates, and extreme values.
2. Investigated the relationship between nutritional, categorical, and serving features and traffic.
3. Imputed missing numeric values using category-level medians.
4. Encoded categorical variables and created train/test datasets.
5. Trained logistic-regression and random-forest classifiers.
6. Compared the models using accuracy, precision, recall, F1 score, and confusion matrices.
7. Selected a model according to the stated business objective.

## Limitations

- The dataset is small and represents a single snapshot of content performance.
- High traffic may be affected by placement, promotion, seasonality, photography, and audience trends that are not included in the data.
- Missing high-traffic labels are interpreted as negative outcomes based on the dataset's encoding convention.
- Nutritional inconsistencies reduce confidence in those features.
- Category relationships are observational and do not establish that selecting a particular category causes more traffic.
- Model performance should be confirmed on a future time-based holdout set before production deployment.

## Repository Contents

| File | Description |
|---|---|
| [`Recipe_Site_Traffic.ipynb`](Recipe_Site_Traffic.ipynb) | Data validation, exploratory analysis, model development, evaluation, and recommendations |

The source dataset is not included in this repository. The notebook retains its analytical outputs but requires the original local dataset to rerun.

## Tools

Python, Pandas, NumPy, Matplotlib, Seaborn, scikit-learn, data validation, exploratory data analysis, and binary classification.
