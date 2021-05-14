# WalkthroughProject





<p float="left">
 <img src="https://upload.wikimedia.org/wikipedia/commons/thumb/e/ed/Australia_satellite_plane.jpg/1094px-Australia_satellite_plane.jpg" width="40%" height="40%"/ hspace="40">
 <img src="https://i.pinimg.com/originals/38/74/88/3874881e3752448272bf63f00b63dae0.jpg" width="33%" height="33%"/>
 </p>


--- 

* explore data using notebook and custom streamlit app
* modeling, evaluation
* create streamlit app

---

# Variables Meaning

|    Heading    |                                                 Meaning                                                |        Units        |
|:-------------:|:------------------------------------------------------------------------------------------------------:|:-------------------:|
| Date          | YYYY - MM - DD                                                                                         |                     |
| Location      | Australian city                                                                                        |                     |
| MinTemp       | Minimum temperature in the 24 hours to 9am. Sometimes only known to the nearest whole degree.          | degrees Celsius     |
| MaxTemp       | Maximum temperature in the 24 hours from 9am. Sometimes only known to the nearest whole degree.        | degrees Celsius     |
| RainfallToday | Rainfall levels                                                                                        | milimitres          |
| Evaporation   | "Class A" pan evaporation in the 24 hours to 9am                                                       | millimetres         |
| Sunshine      | Bright sunshine in the 24 hours to midnight                                                            | hours               |
| WindGustDir   | Direction of strongest gust in the 24 hours to midnight                                                | 16 compass points   |
| WindGustSpeed | Speed of strongest wind gust in the 24 hours to midnight                                               | kilometres per hour |
| Temp9am       | Temperature at 9 am                                                                                    | degrees Celsius     |
| Humidity9am   | Relative humidity at 9 am                                                                              | percent             |
| Cloud9am      | Fraction of sky obscured by cloud at 9 am                                                              | eighths             |
| WindDir9am    | Wind direction averaged over 10 minutes prior to 9 am                                                  | compass points      |
| WindSpeed9am  | Wind speed averaged over 10 minutes prior to 9 am                                                      | kilometres per hour |
| Pressure9am   | Atmospheric pressure reduced to mean sea level at 9 am                                                 | hectopascals        |
| Temp3pm       | Temperature at 3 pm                                                                                    | degrees Celsius     |
| Humidity3pm   | Relative humidity at 3 pm                                                                              | percent             |
| Cloud3pm      | Fraction of sky obscured by cloud at 3 pm                                                              | eighths             |
| WindDir3pm    | Wind direction averaged over 10 minutes prior to 3 pm                                                  | compass points      |
| WindSpeed3pm  | Wind speed averaged over 10 minutes prior to 3 pm                                                      | kilometres per hour |
| Pressure3pm   | Atmospheric pressure reduced to mean sea level at 3 pm                                                 | hectopascals        |
| RainToday     | Flag indicating if rained today                                                                        | Yes or No           |
| RainTomorrow  | Flag indicating if it will rain tomorrow                                                               | Yes or No           |
| Latitude      | Latitude                                                                                               |                     |
| Longitude     | Longitude                                                                                              |                     |
| State         | Australian State                                                                                       |                     |



---

# Business Requirements
* As a Data Analyst from CI AgroBusiness division, you are requested by a new Australian customer to provide actionable insights and data driven recommendations on weather information. This new customer has substantial agriculture business in Australia and understanding the rainfall level is critical for their farmer's network. Their clients needs to know precisely if it will rain in the next day.
  * **1** - We want to tell whether or not will rain in the next day in almost 50 Australian cities. In case of rain, you should tell the rainfall level.
  * **2** - We want to cluster the Australian cities and regions according to rainfall levels
  * **3** - We want to understand the rainfall seasonality for a given city in the last 5 years.

---

# Hypothesis and how to validate?
* some region has more rainfall? 
* which region is more difficult to predict
* xxxx

---

# Rationale to map the business requirements to the Data Visualizations and ML tasks
* **Business Requirement 1**: Classification and Regression
  * We build a Classifier (WeatherClf) to predict RainTomorrow based on weather data
  * We will build a Regression Model (WeatherReg) to predict RainfallTomorrow based on weather data

* **Business Requirement 2**: Cluster
  *  We will build a Clustering Model (WeatherClust) to group weather data

* **Business Requirement 3**: Data Visualization
  * We will subset a given city and the data from the last 5 years; then resample and plot a line chart (Rainfall x Time)
  * There wil be 3 plots:
    * Resampled by year
    * Resampled by month
    * Resampled by day

---

# ML Business Case
## WeatherClf
* We want a ML model to predict if it will rain tomorrow. It is a supervised model, a 2-class, single-label, classification model: 0 (no), 1 (yes)
*  Our ideal outcome is provide to our farmer's network a reliable insight if it will rain or not tomorrow, so they can plan their immediate demand.
*  The model success metrics are
    *  at least 90% Recall, Precision and F1 Score for RainTomorrow = 1 (yes), on train and test set
    *  The ML model is considered a failure if:
       *  after 1 month of usage, it fails to predict accurately more than 70% (which is the current accuracy level from local TB news)
       *  F1 score for RainTomorrow = 1 (yes), on test set
* The output is defined as flag, indicating if it will rain tomorrow or not. This conclusion is made based on the chance of raining tomorrow. If it is more than 50%, we say it will rain. The farmer will access the App, and the location  (city, gps coordinates) will be pulled automatically. After 3pm, we will kbe able to pull the data for all features from local weather station. Once these information are pulled, the App will be ready for the prediction. The live data depends on GPS and local weather station, which are pulled from external API. The prediction is made on the fly (not in batches)
* Heuristics: If we didnt use ML, an alternative option, which is the current option, is to rely on predictions for raining next day from local news, which traditionally over the last 10 years, has an accuracy of 70%.
* The training data to fit the model come from Australian Government - Bureau of Meteorology. This dataset contains about 10 years of daily weather observations from many locations across Australia.
    * Train data - label: RainTomorrow ; features: all other variables:

## WeatherReg
* We want a ML model to predict tomorrow's rainfall levels, in mm. It is a supervised model, a uni-dimensional regression model.
* Our ideal outcome is provide to our farmer's network a reliable insight on tomorrow's rainfall level, so they can plan their immediate demand.
*  The model success metrics are
    *  at 0.85 for R2 score, on train and test set
    *  The ML model is considered a failure if:
       *  after 2 months of usage, model's predictions are 50% off more than 30% of the time. Say, a prediction is >50% off if predicted 10mm and the actual value was >15mm. If this behaviour happens more than 30% in 2 months, the model has failed
* The output is defined as a continious value for rainfall, in mm. It is assumed that this model will predict tomorrow rainfall level if the WeatherClf model predicts 1 (yes for rain). The farmer will access the App, and the location  (city, gps coordinates) will be pulled automatically. After 3pm, we will kbe able to pull the data for all features from local weather station. Once these information are pulled, the App will be ready for the prediction. The live data depends on GPS and local weather station, which are pulled from external API. The prediction is made on the fly (not in batches)
* Heuristics: If we didnt use ML, an alternative option, which is the current option, is to rely on predictions for raining next day from local news, which traditionally over the last 10 years, has an accuracy of 70%.
* The training data to fit the model come from Australian Government - Bureau of Meteorology. This dataset contains about 10 years of daily weather observations from many locations across Australia.
    * Train data - subset RainfallTomorrow as 1, label: RainfallTomorrow, features: all other variables

## WeatherClust
* We want a ML model to cluster rainfall levels. It is a unsupervised model
* Our ideal outcome is provide to our farmer's network a reliable insight on how similar are rainfall levels across Australia
*  The model success metrics are
    *  at least 0.5 for silhoute socre
    *  The ML model is considered a failure if:
       *  model suggests from than 15 clusters (might become too difficult to interpret in practical terms)
* The output is defined as an additional column appended to the dataset. This column represents the clusters suggestions. It is a categorical and nominal variable, represented by numbers, starting at 0.
* Heuristics: If we didnt use ML, an alternative option, would be to rely on geographical encyclopedias made in the 90's 
* The training data to fit the model come from Australian Government - Bureau of Meteorology. This dataset contains about 10 years of daily weather observations from many locations across Australia.
    *  Train data - features: all variables

---

# Dashboard Design

## Streamlit App User Interface

### Page 1: Rainfall prediction
* User Interface with inputs (city and weather data) and prediction indicating the chance of raining tomorrow. If it is greater than 50%, it will rain. In that case, it tells, how much it would rain

### Page 2: Rainfall Seasonality
* User Interface with menu to select a city. 
* 3 Line chart plots indicating, respectively, the seasonality for Year, Month and Day.

### Page 3: WeatherClf
* Evaluation metrics/performance on WeatherClf
  * For both train and test set: Confusion Matrix and Classification Report
  * Bias/Variance Tradeoff

### Page 4: WeatherReg
* Evaluation metrics/performance on WeatherReg
  * For both train and test set: R2, RMSE, MSE, MAE
  * Bias/Variance Tradeoff

### Page 5: WeatherClust
* Evaluation metrics/performance on WeatherClust
  * 3D Scatter Plot for PCA with 3 components, colored by clusters
  * Silhouete score

---

## Django App User Interface

* It contains **Page 1** and **Page 2** from Streamlit App User Interface
