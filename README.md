# **Digital Phenotyping Pipeline for Plant Growth and Yield Prediction**
## **Project Overview**
This project simulates and analyzes plant growth dynamics using synthetic multi-modal sensor data inspired by modern greenhouse phenotyping systems.
The goal is to understand how environmental conditions and imaging-derived features influence plant growth over time, to predict final yield from early-stage signals, and to extract interpretable biological insights using explainable AI.
This work reflects analytical approaches used in digital agriculture and automated phenotyping platforms.

## **Objectives**

Simulate realistic plant phenotyping datasets (imaging, environment, stress)
Explore relationships between environmental conditions and plant growth
Build a machine learning model to predict yield from early-stage sensor data
Interpret model predictions using explainable AI (SHAP)

## **Methods**
**1. Synthetic data generation**
A biologically informed simulation was created where:

Growth depends on temperature, light, and treatment
Sensor features are correlated with plant development
Noise is added to mimic real-world variability
Red and NIR images are simulated at the pixel level to compute plant-level NDVI via the NDVISimulator class


**2. Exploratory Data Analysis (EDA)**
Growth trajectories were analyzed across 30 days per plant, with a focus on:

Growth dynamics across light conditions — comparing plant height trajectories under different light levels
Environmental effects on growth — correlation matrix across height, leaf area, NDVI, canopy volume, water stress, temperature, humidity, CO₂, and light intensity
K-means clustering — plants were clustered (k=3) based on their 30-day height trajectories; PCA was used to visualize cluster separation in 2D
Post-hoc cluster interpretation — mean NDVI, water stress index, and canopy volume compared across clusters
NDVI pixel-level visualization — high-NDVI and low-NDVI plant examples simulated and compared across Red channel, NIR channel, and NDVI maps
Treatment effect on growth — growth trajectory comparison between control and fertilizer treatment groups


**3. Yield prediction model**
Final yield was defined as a weighted composite of day-30 values:
yield = 0.6 × height + 0.3 × canopy_volume + 0.4 × ndvi − 0.2 × water_stress_index + noise
A Random Forest Regressor was trained using early-stage features (days 1–15), aggregated per plant:

height, leaf area, NDVI, canopy volume (mean)
temperature, humidity, CO₂, light intensity, water stress index (mean)

Model performance is evaluated using R² and MAE on a held-out test set.

**4. Model explainability with SHAP**
SHAP (SHapley Additive exPlanations) was applied to the Random Forest model to:

Quantify the contribution of each feature to yield prediction (summary plot and bar plot)
Interpret individual plant predictions using SHAP force plots


## **Key Insights**

- NDVI is strongly positively associated with plant height and overall growth
- Growth-related variables (height, leaf area, canopy volume) are highly intercorrelated, indicating consistent structural development
- Water stress shows a negative correlation with both NDVI and growth variables, confirming its inhibitory effect on plant development
- Environmental variables (temperature, light intensity) show moderate associations, suggesting their effects are mediated through plant physiology rather than direct influence
- K-means clustering reveals distinct growth behavior groups (e.g., fast-growing, slow-growing, plateauing); clusters with higher NDVI, lower water stress, and larger canopy volume correspond to taller plants
- SHAP analysis identifies plant structure and health (height, canopy volume, NDVI) as the dominant drivers of yield prediction, with CO₂ providing a smaller positive contribution