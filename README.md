## Power Outage Duration Prediction Project

### Prediction Problem and Type
- **Type:** Regression  
- **Response Variable:** Duration of Power Outage

### Project Objective
The primary objective is to predict the duration of power outages using a comprehensive dataset. This prediction is crucial for enabling prompt and effective response strategies. By estimating outage durations more accurately, utility companies and emergency services can optimize their resource allocation and minimize the overall impact on affected areas.

### Dataset
Our analysis utilizes historical outage data collected since 2012. This dataset is rich in features that potentially influence outage durations. After thorough data cleaning, the following key attributes are retained for model training:
- `NERC.REGION` (NERC Region)
- `CLIMATE.REGION` (Climate Region)
- `ANOMALY.LEVEL` (Anomaly Level)
- `CLIMATE.CATEGORY` (Climate Category)
- `CAUSE.CATEGORY` (Cause Category)
- `CUSTOMERS.AFFECTED` (Number of Customers Affected)
- `POPULATION` (Population)
- `U.S._STATE` (U.S. State)
- `CAUSE.CATEGORY.DETAIL` (Cause Category Detail)
- `PI.UTIL.OFUSA` (PI Utility of USA)
- `OUTAGE.DURATION` (Outage Duration)

### Model Evaluation Metric
- **Primary Metric:** Mean Absolute Error (MAE)
- **Rationale:** MAE is chosen as the primary evaluation metric for its direct interpretability and robustness against outliers. Unlike metrics like Mean Squared Error, MAE does not overly penalize large errors, which is beneficial in our dataset that exhibits a significant number of outliers and irregular patterns. This makes MAE a more reliable measure of central tendency in predicting outage durations, providing a more realistic assessment of model performance.


### Data Preprocessing
Following procedures from our data cleaning, we converted the dataset from XLSX to CSV. We dropped irrelevant rows and columns, retaining only features correlated with `OUTAGE.DURATION`.

| OBS | NERC.REGION | CLIMATE.REGION      | ANOMALY.LEVEL | CLIMATE.CATEGORY | CAUSE.CATEGORY     | CUSTOMERS.AFFECTED | POPULATION | U.S._STATE | CAUSE.CATEGORY.DETAIL | PI.UTIL.OFUSA | OUTAGE.DURATION |
|-----|-------------|---------------------|---------------|------------------|--------------------|--------------------|------------|------------|-----------------------|---------------|-----------------|
| 1.0 | MRO         | East North Central  | -0.3          | normal           | severe weather     | 70000.0            | 5348119.0  | Minnesota  | NaN                   | 2.2           | 3060            |
| 2.0 | MRO         | East North Central  | -0.1          | normal           | intentional attack | NaN                | 5457125.0  | Minnesota  | vandalism             | 2.2           | 1               |
| 3.0 | MRO         | East North Central  | -1.5          | cold             | severe weather     | 70000.0            | 5310903.0  | Minnesota  | heavy wind            | 2.1           | 3000            |
| 4.0 | MRO         | East North Central  | -0.1          | normal           | severe weather     | 68200.0            | 5380443.0  | Minnesota  | thunderstorm          | 2.2           | 2550            |
| 5.0 | MRO         | East North Central  | 1.2           | warm             | severe weather     | 250000.0           | 5489594.0  | Minnesota  | NaN                   | 2.2           | 1740            |
| ... | ...         | ...                 | ...           | ...              | ...                | ...                | ...        | ...        | ...                   | ...           | ...             |



## Baseline Model Overview and Performance Analysis for Power Outage Duration Prediction

### Baseline Model Description
The baseline model was established to set a foundational understanding of the problem and provide a benchmark for future model iterations.

- **Model:** Linear Regression
- **Features Used in the Model:**
  - `ANOMALY.LEVEL` (Quantitative)
  - `CAUSE.CATEGORY` (Nominal)

  There are no ordinal features in this baseline model.

### Baseline Model Features and Encoding

#### Features
- **`ANOMALY.LEVEL`:** This is a quantitative feature that indicates the anomaly level, typically reflecting deviations from normal weather or climatic conditions. It is a numerical variable that can provide insight into the severity or uniqueness of the conditions leading to a power outage.

- **`CAUSE.CATEGORY`:** This is a nominal feature that describes the category of the cause of the power outage, such as natural disasters, equipment failure, or human error. It is a categorical variable that helps in understanding the primary factors behind power outages.

- **`OUTAGE.DURATION(mins)`:** This quantitative feature represents the duration of the power outage in minutes. It is the response variable in the model and is a crucial numerical indicator of the severity and impact of the outage.

#### Encoding
- **Categorical Feature Encoding:**
  - The model uses one-hot encoding for the `CAUSE.CATEGORY` feature. This technique converts the categorical data into a binary numerical format, with separate columns representing each unique category in the dataset.
  
- **Numerical Feature Handling:**
  - The `ANOMALY.LEVEL` feature, being numerical, does not require categorical encoding. However, it undergoes standard scaling to ensure that its values have a consistent scale compared to transformed categorical features.

- **ColumnTransformer Configuration:**
  - The `ColumnTransformer` in the model's pipeline is set up with the `remainder='passthrough'` parameter. This setup ensures that while the categorical `CAUSE.CATEGORY` undergoes one-hot encoding, the numerical `ANOMALY.LEVEL` feature is left unchanged, preserving its original form.


### Model Performance
- **Evaluation Metric:** Mean Absolute Error (MAE)
- **MAE on Test Data:** 2666.51

  This metric provides a baseline understanding of the model's performance on predicting outage durations.

### Assessment of Baseline Model
- **Current Model Effectiveness:**
  - The baseline model offers a fundamental level of prediction capability. However, its simplicity also limits its accuracy and predictive power.
  - The model only incorporates two features, which might not capture the complexity and multifaceted nature of power outage durations.
  - The relatively high MAE suggests there is substantial room for improvement.

- **Good or Not?**
  - The model can be considered "adequate" for establishing a baseline but not "good" in terms of providing high accuracy predictions.
  - The simplicity of the model makes it a good starting point but not sufficient for capturing the nuanced relationships in the data.

### Conclusion
While the baseline model sets an initial benchmark, it underscores the need for more sophisticated modeling approaches and a broader set of features to improve prediction accuracy and gain deeper insights into power outage duration factors.



## Final Model Description and Performance Analysis

### Features in the Final Model
In the final model, we carefully selected a blend of features that offer comprehensive insights into the factors influencing power outage durations:

- **Added Features:**
  - `NERC.REGION`
  - `CLIMATE.REGION`
  - `ANOMALY.LEVEL`
  - `CLIMATE.CATEGORY`
  - `CAUSE.CATEGORY`
  - `CUSTOMERS.AFFECTED`
  - `POPULATION`
  - `U.S._STATE`
  - `CAUSE.CATEGORY.DETAIL`
  - `PI.UTIL.OFUSA`

 ### Explanation of Features in the Model

- **NERC.REGION**: This feature is transformed using one-hot encoding. The North American Electric Reliability Corporation (NERC) region provides essential geographical information that can influence power outage durations. Different regions may have unique infrastructure, environmental, and operational characteristics affecting outage patterns.

- **CLIMATE.REGION**: Also transformed using one-hot encoding, this feature categorizes the geographic area into specific climate regions. Climate regions are critical in understanding and predicting outage durations since certain weather patterns and environmental conditions are unique to each region.

- **ANOMALY.LEVEL**: Scaled using StandardScaler, the anomaly level indicates deviations from typical climatic conditions, which might be a crucial factor in the occurrence and duration of power outages. Scaling ensures this numeric feature is appropriately normalized for the model.

- **CLIMATE.CATEGORY**: Transformed using one-hot encoding, this feature provides insights into the broader climatic conditions of a region, such as tropical, arid, or temperate climates. These categories can significantly impact the likelihood and severity of power outages.

- **CAUSE.CATEGORY**: Transformed using one-hot encoding, this feature identifies the primary cause of the power outage, such as natural disasters, technical failures, or human errors. Understanding the cause is vital for predicting outage durations and developing mitigation strategies.

- **CUSTOMERS.AFFECTED**: Scaled using StandardScaler, this feature quantifies the number of customers impacted by an outage. It's an important indicator of the outage's scale and can correlate with the duration and severity of the outage.

- **POPULATION**: Scaled using StandardScaler, this feature reflects the population size of the affected area. Higher population areas might have more complex infrastructure, potentially influencing the duration of outages.

- **U.S._STATE**: Transformed using one-hot encoding, this feature allows the model to capture state-specific factors that could affect outage durations, such as state-level policies, infrastructure quality, and geographic factors.

- **CAUSE.CATEGORY.DETAIL**: Transformed using one-hot encoding, this feature provides more granular details about the cause of the outage. This finer level of categorization can help in understanding specific conditions leading to outages.

- **PI.UTIL.OFUSA**: Scaled using StandardScaler, this feature represents the utility performance index of the USA. It provides an overview of the overall condition and reliability of the power infrastructure, which can be a crucial factor in the duration and frequency of power outages.


### Modeling Algorithm and Hyperparameters
- **Model:** RandomForestRegressor
- **Hyperparameters and Selection Method:**
  - We utilized GridSearchCV for hyperparameter tuning, evaluating various combinations to find the optimal settings.
  - **Best Performing Hyperparameters:**
    - `max_depth`: None
    - `min_samples_leaf`: 2
    - `min_samples_split`: 10
    - `n_estimators`: 100

  RandomForestRegressor was chosen for its effectiveness in handling a mix of categorical and numerical features and its ability to model complex, non-linear relationships inherent in the data.

### Performance Comparison with Baseline Model
- **Baseline Model Performance:** The baseline model, employing Linear Regression with a limited set of features (`ANOMALY.LEVEL`, `CAUSE.CATEGORY`), resulted in a Mean Absolute Error of 2666.51 on the test data.
- **Final Model Performance:** 
  - **Mean Absolute Error (MAE):** 1971.70
  - This represents a substantial improvement in predictive accuracy compared to the baseline model.
  
  The significant reduction in MAE can be attributed to the more sophisticated RandomForestRegressor algorithm, the expanded and more informative set of features, and the optimized hyperparameters.

### Conclusion
The final model's superior performance over the baseline model is a testament to the effective combination of feature selection, modeling strategy, and hyperparameter optimization. It showcases a better understanding and capturing of the complexities involved in predicting power outage durations.


## Fairness Analysis of Power Outage Duration Prediction Model

### Group Definitions
- **Group X (Normal Climate Category):** This group consists of data points where the `CLIMATE.CATEGORY` is labeled as 'normal'.
- **Group Y (Warm and Cold Climate Categories):** This group includes data points where the `CLIMATE.CATEGORY` is either 'warm' or 'cold'.

### Evaluation Metric
- **Metric Used:** Root Mean Squared Error (RMSE)
- RMSE is chosen as it effectively captures the average magnitude of the errors in a prediction, making it suitable for a regression task like ours.

### Hypotheses
- **Null Hypothesis (H0):** The model's performance, in terms of RMSE, is similar for both normal climate categories and warm/cold climate categories. Any observed differences are due to random chance.
- **Alternative Hypothesis (H1):** The model's performance is significantly different between the two climate categories, suggesting a disparity in prediction accuracy.

### Test Statistic and Significance Level
- **Test Statistic:** Difference in RMSE between Group X and Group Y.
- **Significance Level:** Typically, a significance level (α) of 0.05 is used.

### Permutation Test
- **Procedure:** The permutation test involved shuffling the `CLIMATE.CATEGORY` values and recalculating the RMSE for 1000 permutations.
- **Observed Difference in RMSE:** 410.79
- **P-value:** 0.437

### Conclusion
- Given the p-value of 0.437, which is greater than our significance level of 0.05, we fail to reject the null hypothesis. This suggests that the observed difference in RMSE between normal and warm/cold climate categories could be due to random variation.
- In other words, there is no strong statistical evidence to conclude that the model's performance is significantly different across these climate categories.
- It is important to remember that these results do not definitively prove the null hypothesis; rather, they indicate that our current dataset does not provide sufficient evidence to support the alternative hypothesis of model bias between these groups.

### Visualization


