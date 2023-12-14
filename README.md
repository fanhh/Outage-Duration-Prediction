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

### Feature Encoding and Preprocessing
- **Quantitative Feature (`ANOMALY.LEVEL`):**
  - No specific encoding was required. Standard scaling was applied to normalize this feature.
- **Nominal Feature (`CAUSE.CATEGORY`):**
  - Encoded using OneHotEncoder. This transformation is essential for converting categorical text data into a machine-readable numeric format without implying any order.

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

  **Rationale for Selection:**
  - These features encompass a wide range of factors, from geographical (e.g., `NERC.REGION`, `U.S._STATE`) and climatic (`CLIMATE.REGION`, `ANOMALY.LEVEL`) to demographic (`POPULATION`, `CUSTOMERS.AFFECTED`) and operational aspects (`CAUSE.CATEGORY`, `PI.UTIL.OFUSA`).
  - Each feature is expected to have a distinct impact on the duration of power outages. For example, `ANOMALY.LEVEL` might be indicative of extreme weather conditions, while `CUSTOMERS.AFFECTED` gives an idea about the scale of the outage.

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




