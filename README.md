# Capstone2-GithubSubmission 
## 1. Mycotoxin Risk Prediction
### 2. Problem/Motivation
Mycotoxins are toxins produced by fungi that colonize food crops and can cause serious economic burden to the US corn industry. Aflatoxins may occur in food because of mold growth in susceptible raw agricultural products. The growth of molds that produce aflatoxins is influenced by environmental factors such as temperature, humidity, and extent of rainfall during the pre-harvesting, harvesting, or post-harvesting periods (FDA). It has been estimated that aflatoxin contamination could cause losses ranging from $52.1 million to $1.68 billion annually in the United States (Castano-Duque et al). It has been proven that healthier plants are less at risk. Also, plants are more at risk in hot and dry conditions. Pre-and post-planting weather factors are significantly correlated with Aflatoxin and Fumonisin contamination at harvest which is why I gathered weather data from six months before and after the planting dates given in my datasets.
Aflatoxins are one of the most problematic because of their carcinogenic and mutagenic properties that can contaminate food and feed. The FDA says that human food containing total aflatoxins greater than 20 micrograms per kilogram, or ppb may be injurious to health. Consumption of food with high levels of aflatoxins is also associated with liver cancer in humans. 
For my Capstone project I trained a model to predict the Aflatoxin contamination at harvest using weather data. In Capstone 1 I created a baseline model and a linear regression model. 
### 3. Project Overview

### 4. Data
Source:  
https://agpmt.org/data-management/   

Type:  
CSV  

Size:  
(1048576, 37)  

Key features:  
Location, NOLA_ID, Planting_Date, Harvest_Date, GPS, Temperature, Precipiation, NDVI  

### 5. Data Preprocessing
#### Cleaning Steps:
I had to make sure each row had a PLanting_Date, Harvest_Date, and GPS coordinates otherwise those rows were not usefull.
I collected the weather data hourly at first and then daily and weekly.
#### Handling missing values:
The only weather feature with missing values was NDVI since I used a quailty assurance mask to only use values that were valid. This means when it was very cloudy or there was snow that NDVI was not included in my data.
#### Feature engineering:
GPS was  split into Latitude and Longitude. Aflatoin Risk Index code was used in R to calculate that weekly value as well as features like Growing Degree Days and Cumulative Growing Degree Days.

### 6. Exploratory Data Analysis
<img width="612" height="363" alt="image" src="https://github.com/user-attachments/assets/d7b5a4f6-23d1-44ac-b51b-ed74528cf8df" />  
Locations of GPS coordinated for each year.  
  

<img width="285" height="169" alt="image" src="https://github.com/user-attachments/assets/70fc8970-f446-4375-9723-c2bc3fba6f56" />  
Aflatoxin contamination results at harvest.
  

<img width="570" height="510" alt="image" src="https://github.com/user-attachments/assets/8fbad414-2b3c-4af2-9e05-b769c3919596" />  
Example of NDVI.
  

<img width="462" height="413" alt="image" src="https://github.com/user-attachments/assets/6f08668f-a277-4108-826e-7174f31ed61f" />  
Confustion matrix with only weather features. 
  

<img width="400" height="358" alt="image" src="https://github.com/user-attachments/assets/6a1e4c94-92b7-426a-b060-1ef249f2f7c2" />
Confusion Matrix with ARI features. The correlation matrix shows that no single variable strongly explains aflatoxin contamination.
However, strong correlations exist between engineered features such as GDD, growth variables, and ARI, indicating redundancy.
This suggests that aflatoxin prediction depends on nonlinear interactions between environmental and biological factors, rather than simple linear relationships.  
  
<img width="834" height="54" alt="image" src="https://github.com/user-attachments/assets/5b4463f6-2a34-4be8-b184-931e4e6b1049" />
  

### 7. Modeling Approach
Baseline model:
Random Forest using GroupKFold with 5 folds by Location.
I used grouped cross-validation by location to avoid spatial leakage. This ensures the model is tested on locations it has not seen before. Results are an RMSE of 2.30, MAE of 0.64, and an R² of -0.09. The negative R² suggests that predicting aflatoxin across different locations is challenging, likely due to strong environmental and regional differences.

One advanced model:  
I moved on to a two-stage approach. The first stage being a classification model to determine contamination or no contamination. Stage two is a regression mode to preditc aflatoxin amount only for those contaminated cases.

Why I chose them:  
It is important to note that my dataset is about 90% zeroes in the aflatoxin target, meaning 90% of the crops were not contaminated. When I evaluated the baseline random forest model specifically on non-zero aflatoxin cases, the error increased substantially. This shows that while the model handles the majority of zero cases well, it struggles to predict actual contamination events. This highlights the need for a different modeling strategy, the two-stage approach.


### 8. Model Training 
Tools used
Hyperparameters:  
Starting at a default threshold of 0.5 with the regression model gave me   
RMSE: 1.6887343388777052  
MAE: 1.6695052547403957  
R²: -4.507769503825259e+30  
When I changed the threshold to 0.35, which is appropriate for this project where missing contamination is more hurtful than false positives. My results then greatly improved and my results overall were.  
RMSE: 0.8799  
MAE : 0.2648  
R²  : 0.3875  
And for non-zero only:  
RMSE: 1.6258  
MAE : 0.8014  
R²  : 0.2799  

### 9. Results
To evaluate the models, I used both classification and regression metrics because the project has two goals. First, predicting whether aflatoxin contamination occurs then estimating the aflatoxin level.
For the classification model I used:  
Accuracy: Measures the overall percentage of correct predictions. However, because aflatoxin data can be imbalanced, accuracy alone is not enough.
Precision: Measures how many predicted contamination cases were actually contaminated. This is important because false alarms could lead to unnecessary concern or extra testing.
Recall: Measures how many true contamination cases the model successfully identified. This is especially important because missing a contaminated sample could have serious agricultural and food safety consequences.
F1-score: Balances precision and recall, making it useful when the dataset has uneven classes.
ROC-AUC: Measures how well the model separates contaminated and non-contaminated samples across different thresholds.
For the regression model, I used:
MAE: Measures the average size of prediction errors. It is easy to interpret because it is in the same general scale as the target variable.
RMSE: Penalizes larger errors more strongly, which is useful because very high aflatoxin levels are especially important to predict.
R²: Shows how much variation in aflatoxin levels the model explains.



  Baseline Models Comparison Table:   
<img width="425" height="105" alt="image" src="https://github.com/user-attachments/assets/731bbdbc-6f06-4f19-95b0-eb7321434a5e" />
<img width="389" height="214" alt="image" src="https://github.com/user-attachments/assets/57d2cc72-145d-423a-90e0-1120f44b3a48" />
<img width="514" height="365" alt="image" src="https://github.com/user-attachments/assets/bc4421bd-63ba-42c2-a186-f4593f823d00" />



### 10. Model Interpretation
Model interpretation was performed using feature importance and SHAP values. These methods helped identify which variables had the strongest influence on aflatoxin prediction.
Important features included variables such as NDVI, temperature, precipitation, relative humidity, GDD, cumulative GDD, and days from planting. These features are biologically meaningful because aflatoxin contamination is affected by plant stress, weather conditions, crop development, and environmental moisture.
SHAP plots were especially useful because they showed not only which features mattered most, but also how high or low values of those features influenced the model predictions.  
<img width="346" height="232" alt="image" src="https://github.com/user-attachments/assets/5c975f0a-bb75-47da-ab3a-d19a68ba19cb" />
<img width="390" height="440" alt="image" src="https://github.com/user-attachments/assets/3cb486e9-39ec-43f9-b933-af69b66a5d7d" />

### 11. Key Insights  
The best performing models were tree based machine learning models such as Random Forest, XGBoost, or LightGBM because they can capture nonlinear relationships between weather, vegetation indices, crop timing, and aflatoxin risk.
Aflatoxin prediction is difficult because contamination is affected by many interacting biological and environmental factors. The dataset also contains many zero or contamination samples, which makes prediction more challenging.
The practical impact of this project is that it could help identify fields or locations with higher aflatoxin risk before harvest. This can support better monitoring, targeted testing, and earlier decision-making for crop management and food safety.
  

### 12. Conclusion
In this project I developed a machine learning workflow to predict aflatoxin contamination using environmental, remote sensing, and agronomic data. Multiple models were compared using classification and regression metrics. The results showed that tree-based models were useful for identifying important risk factors and predicting contamination patterns.
Overall, the project demonstrates how data science, remote sensing, and agricultural data can be combined to support aflatoxin risk prediction and help farmers.

### 13. Future Work

Future improvements could include testing more advanced models such as neural networks. Also xpanding SHAP interpretation and editing models based on that information. Deploying the final model in a Streamlit app for farmers is a final step that would require exploring predicting weather features.
  
### 14. How to Run
Install dependencies  
Run preprocessing  
Train model  
Evaluate results  
  
### 15. Repository Structure Explanation
AggregateMetadata.ipynb - Join the data from 2023 and 2024 keeping in mind that GPS locations change each each even if Location matches.  
AddWeatherNDVI.ipynb - Get weather features using point like temperature, precipitation, humidity and others frm the NLDAS satellite.  
CountyWeather.ipynb - Get weather features using country like temperature, precipitation, humidity and others frm the NLDAS satellite. County is for anonymity and to check that there is not weather phenomenons occuring at point locations.  
Sanity_checks.ipynb - Check for missing values and graph weather to features to check for following of seasons. For example, hottest in summer and coldest in winter. Higher precipition and NDVI in spring.  

### 16. Requirements
pandas
numpy
scikit-learn
matplotlib
seaborn
joblib
streamlit
shap
xgboost
lightgbm
earthengine-api
geemap
geopandas
