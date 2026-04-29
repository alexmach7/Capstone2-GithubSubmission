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
Baseline model
One advanced model
Why I chose them

### 8. Model Training 
Tools used
Hyperparameters
Training process

### 9. Results
Metrics (Clearly defined metrics(with explanation of why they were chosen))
Model comparison table
Visualization

### 10. Model Interpretation

### 11. Key Insights
- What worked best and why
- What is the practical or business impact of your results?

### 12. Conclusion
- Final summary


### 13. Future Work
- Improvements and next steps

  
### 14. How to Run
- Install dependencies
- Run preprocessing
- Train model
- Evaluate results
  
### 15. Repository Structure Explanation
Explain each file clearly

### 16. Requirements
pip install -r requirements.txt
