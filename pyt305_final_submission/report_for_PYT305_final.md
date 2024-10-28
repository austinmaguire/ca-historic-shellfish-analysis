# Analysis of California Shellfish Landings: A Machine Learning Approach

## Final Project for PYT305 - Introduction to Machine Learning with Python

### _Prepared by - Austin Maguire_

## 1. Introduction

This project analyzes historical California shellfish landing data to predict fishing success using machine learning techniques. The analysis focuses on understanding patterns in shellfish landings across different species groups, ports, and temporal scales, with the ultimate goal of developing predictive models for fishing success.

### 1.1 Research Questions

1. Can we predict fishing success based on temporal and spatial patterns?
2. How do seasonal patterns affect different species groups?
3. Which features are most important in predicting fishing success?
4. How do different ports vary in their species composition and landing patterns?
5. Can machine learning models effectively categorize fishing success levels?

## 2. Data Preparation and Cleaning

### 2.1 Dataset Overview

The original dataset contains California shellfish landing records with information about:

- Species
- Port of landing
- Landing volumes
- Date of landing
- Year of landing

### 2.2 Data Cleaning Steps

1. Renamed 'fish' column to 'species' for clarity
2. Converted time column to pandas datetime format
3. Created a new 'month' column for temporal analysis
4. Removed zero-landing records
5. Removed 'All' port entries to avoid data duplication
6. Categorized species into groups:
   - Abalone
   - Clams
   - Crabs
   - Oysters
   - Shrimp & Prawns
   - Other

### 2.3 Feature Engineering

1. Created 'monthly_mean_landing' by species group and port
2. Developed 'fishing_success' categorical variable using quartile-based classification:
   - Poor (Q1)
   - Fair (Q2)
   - Good (Q3)
   - Excellent (Q4)

Engineering and refining the 'monthly_mean_landing' and 'fishing_success' features ended up occupying a fair amount of the data preprocessing so I'll address the thought process of its development.

1. First Approach - Monthly Means Across Years:

- Calculated 'monthly_mean_landing' by averaging landings for each species and month across all years
- This meant January abalone catches were compared against all January abalone catches in the dataset
- Problem: This didn't account for port-specific variations or overall changes in fishery patterns over time

2. Second Approach - Port-Specific Monthly Means:

- Refined 'monthly_mean_landing' to be more specific:

```python
shellfish_landings['monthly_mean_landing'] = shellfish_landings.groupby(
    ['year', 'month', 'species', 'port'])['landings'].transform('mean')
```

- This gave the the mean landing for each species, at each port, for each specific month and year
- Better captured local variations and temporal patterns
- Challenge: Some species had too few observations for reliable quartile categorization

3. Final Approach - Species Group Monthly Means:

- Grouped similar species (e.g., all abalone species, all crab species) to create 'species_group'
- Calculated 'monthly_mean_landing' using these broader groups:

```python
shellfish_landings['monthly_mean_landing'] = shellfish_landings.groupby(
    ['year', 'month', 'species_group', 'port'])['landings'].transform('mean')
```

- Created 'fishing_success' categories (poor, fair, good, excellent) based on quartiles of these group means
- Advantages:
  - More observations per group for better statistical reliability
  - More robust categorization for machine learning
  - Better captures overall fishing conditions for similar species

The final approach provides a more balanced categorical variable that should be more useful for machine learning, as it:

1. Has enough data points per category for training
2. Captures real-world fishing patterns at the port level
3. Groups similar species that likely respond to similar environmental and seasonal conditions
4. Creates approximately normally distributed categories through quartile-based classification

It will take a few more investigations into this dataset and additional learning, or perhaps just a few weeks away from the process, to determine how effective this methodology for creating a categorical variable was.

## 3. Exploratory Data Analysis

### 3.1 Species Group Analysis

Early on in the EDA process it was apparent that the largest proportion of landings by weight was for the Crab species group, and for Dungeness Crab specifically. Table 2(e) - Percentage of Total Landings by Species Group, shows that Crab made up 52% of landings by species group. Shrimp and prawns following with 23%. Abalone was 14% of landings, oysters were 9%, and clams only 0.4%.

### 3.2 Temporal Patterns

The most significant temporal patterns were with the Crabs and the Shrimp & Prawns group. Crabs had a steady decline toward October(month 10), and then rose sharply to peak in December. Since there is a season for Dungeness, the predominant species in the crab group, this seems reasonable. The Shrimp & Prawns group interestingly showed a steady increase in summer months, peaking around July, before steadily decreasing into the winter. This is probably due to the seasonal abundance of the stock.

### 3.3 Spatial Analysis

Certain species groups were landed more commonly at certain ports during certain periods, with Santa Barbara landing most of the abalone historically and Eureka consistently landing most of the crab.

## 4. Machine Learning Models

### 4.1 Initial Regression Approaches

Before creating the 'fishing_success' categorical variable I tried running a linear regression and ARIMA time series, both of which performed poorly and led me to pursue a classifier model instead. This is likely due to the significant seasonal variability in the landing of all species groups (outside of clams which never had much a catch share and thus little variance).

- Linear Regression
  - R-squared: 0.16
  - Poor performance due to high variability in landing volumes
- ARIMA Time Series
  - RMSE approximately 3x higher than linear regression
  - Temporal patterns proved too complex for this approach

### 4.2 Classification Models

#### First Iteration

- Random Forest: 45.23% accuracy
- Naive Bayes: 30.48% accuracy
- Feature Importance:
  - Temporal features (Year, Month): ~80%
  - Spatial and species features: ~10% each

#### Enhanced Feature Engineering

- Added seasonal groupings
- Implemented cyclical month encoding
- Created port-season interaction features
- Results:
  - Random Forest: 42.02% accuracy
  - Naive Bayes: 30.06% accuracy

### 4.3 Species-Specific Model (Crab Group)

- Focused on crab species data
- Maintained balanced class distribution
- Results:
  - Random Forest: 41.22% accuracy
  - Naive Bayes: 27.71% accuracy

## 5. Discussion and Insights

### 5.1 Model Performance Analysis

- Classification approaches outperformed regression methods
- Temporal features proved most significant
- Feature engineering attempts showed complexity of patterns

### 5.2 Limitations

- High variability in landing volumes
- Complex temporal patterns
- Limited environmental data

### 5.3 Takeaways

This was an initial foray into dissecting this dataset that I thought on face value was fairly straightforward. 5 variables - Date, Year, Species, Port, and Landing weight. However, the highly stochastic nature of marine fisheries proved to have a nuanced difficulty, at least for someone in the early stages of learning ML, regression, and classifier techniques. I have a career in fisheries so this should come as no surprise, however I thought that the large scale and sheer volume of observations would cancel out the highly variable nature of fisheries landings. This proved to not be the case.

## 6. Conclusion and Future Work

### 6.1 Key Findings

1. Temporal patterns dominate predictive power
2. Port-specific variations exist but are secondary
3. Species grouping provides more reliable predictions
4. Binary classification more effective than precise volume prediction

### 6.2 Future Work

1. Incorporate environmental data
2. Develop species-specific models
3. Explore deep learning approaches
4. Include economic factors

## 7. References

- **Project Repository**: https://github.com/austinmaguire/ca-historic-shellfish-analysis.git
- Database Website (data access): https://upwell.pfeg.noaa.gov/erddap/tabledap/erdCAMarCatLM.html
- Database Website (information): https://oceanview.pfeg.noaa.gov/las_fish1/doc/names_describe.html
