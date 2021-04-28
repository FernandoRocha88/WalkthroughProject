# WalkthroughProject1


* create business case
*  add data: coordinates from each city,, relate city to state/region
* explore data using notebook and custom streamlit app
* modeling, evaluation
* create streamlit app


# Variables Meaning
* Table Source: [link](http://www.bom.gov.au/climate/dwo/IDCJDW0000.shtml). The nomenclature below is slightly different from the dataset, however it is possible to transfer table's interpretation to the existing dataset.

|    Heading    |      |                                                 Meaning                                                |        Units        |   |
|:-------------:|:----:|:------------------------------------------------------------------------------------------------------:|:-------------------:|---|
| Date          |      | Day of the month                                                                                       |                     |   |
| Day           |      | Day of the week                                                                                        | first two letters   |   |
| Temps         | Min  | Minimum temperature in the 24 hours to 9am. Sometimes only known to the nearest whole degree.          | degrees Celsius     |   |
| Temps         | Max  | Maximum temperature in the 24 hours from 9am. Sometimes only known to the nearest whole degree.        | degrees Celsius     |   |
| Rain          |      | Precipitation (rainfall) in the 24 hours to 9am. Sometimes only known to the nearest whole millimetre. | millimetres         |   |
| Evap          |      | "Class A" pan evaporation in the 24 hours to 9am                                                       | millimetres         |   |
| Sun           |      | Bright sunshine in the 24 hours to midnight                                                            | hours               |   |
| Max wind gust | Dirn | Direction of strongest gust in the 24 hours to midnight                                                | 16 compass points   |   |
| 9 am          | Spd  | Speed of strongest wind gust in the 24 hours to midnight                                               | kilometres per hour |   |
| 9 am          | Time | Time of strongest wind gust                                                                            | local time hh:mm    |   |
| 9 am          | Temp | Temperature at 9 am                                                                                    | degrees Celsius     |   |
| 9 am          | RH   | Relative humidity at 9 am                                                                              | percent             |   |
| 9 am          | Cld  | Fraction of sky obscured by cloud at 9 am                                                              | eighths             |   |
| 9 am          | Dirn | Wind direction averaged over 10 minutes prior to 9 am                                                  | compass points      |   |
| 9 am          | Spd  | Wind speed averaged over 10 minutes prior to 9 am                                                      | kilometres per hour |   |
| 9 am          | MSLP | Atmospheric pressure reduced to mean sea level at 9 am                                                 | hectopascals        |   |
| 3 pm          | Temp | Temperature at 3 pm                                                                                    | degrees Celsius     |   |
| 3 pm          | RH   | Relative humidity at 3 pm                                                                              | percent             |   |
| 3 pm          | Cld  | Fraction of sky obscured by cloud at 3 pm                                                              | eighths             |   |
| 3 pm          | Dirn | Wind direction averaged over 10 minutes prior to 3 pm                                                  | compass points      |   |
| 3 pm          | Spd  | Wind speed averaged over 10 minutes prior to 3 pm                                                      | kilometres per hour |   |
| 3 pm          | MSLP | Atmospheric pressure reduced to mean sea level at 3 pm                                                 | hectopascals        |   |

# Business Requirements
* As a Data Analyst from CI AgroBusiness division, you are requested by a new Australian customer to provide actionable insights and data driven recommendations on weather information. This new customer has substantial agriculture business in Australia and understanding the rainfall level is critical for their farmer's network. Their clients needs to know precisely if it will rain in the next day and an reliable reference for the next 7 days.
  * **1** - We want to tell whether or not will rain in the next day in almost 50 Australian cities. In case of rain, you should tell the rainfall level.
  * **2** - We want to cluster the Australian cities and regions according to rainfall levels
  * **3** - We want to understand the rainfall seasonality for a given city in the last 5 years.

# Hypothesis and how to validate?
* some region has more rainfall? or some region is more difficult to predict
* xxxx
* xxxx


# Rationale to map the business requirements to the Data Visualizations and ML tasks
* Business Requirement 1: Classification and Regression
  * We build a Classifier (WeatherClf) to predict RainTomorrow based on weather data
    * Train data - label: RainTomorrow ; features: all other variables:
  * We will build a Regression Model (WeatherReg) to predict RainfallTomorrow based on weather data
    * Train data - subset RainfallTomorrow as 1, label: RainfallTomorrow, features: all other variables

* Business Requirement 2: Cluster
  *  We will build a Clustering Model (WeatherClust) to group weather data
     *  Train data - features: all variables

* Business Requirement 3: Data Visualization
  * We will subset a given city and the data from the last 5 years; then resample and plot a line chart (Rainfall x Time)
  * There wil be 3 plots:
    * Resampled by year
    * Resampled by month
    * Resampled by day

# ML Business Case
## WeatherClf
* We want a ML model to predict if it will rain tomorrow. It is a 2-class, single-label, classification model: 0 (no), 1 (yes)
*  Our ideal outcome is provide to our farmer's network a reliable insight if it will rain or not tomorrow, so they can plan their immediate demand.
*  The model success metrics are
    *  at least 90% Recall, Precision and F1 Score for RainTomorrow = 1 (yes), on train and test set
    *  The ML model is considered a failure if:
       *  after 1 month of usage, it fails to predict accurately more than 70% (which is the current accuracy level from local TB news)
       *  F1 score for RainTomorrow = 1 (yes), on test set
* The output is defined as the chance of raining tomorrow. If it is more than 50%, we say it will rain. The farmer will access the App, and the location  (city, gps coordinates) will be pulled automatically. After 3pm, we will kbe ableto pull the data for all features from local weather station. Once these information are pulled, the App will be ready for the prediction. The live data depends on GPS and local weather station, which are pulled from external API. The prediction is made on the fly (not in batches)
* Heuristics: If we didnt use ML, an alternative option, which is the current option, is to rely on predictions for rainning next day from local news, which traditionally over the last 10 years, has an accuracy of 70%.
* The training data to fit the model come from Australian Government - Bureau of Meteorology. This dataset contains about 10 years of daily weather observations from many locations across Australia.

## WeatherReg
* ssss

## WeatherClust
* ssss

3. What does your ML project do? (We want a ML model to …)
4. Ideal outcome? 
5. Metrics for success and failure
6. Output: what is, how to use it, how to integrate with current processes?
7. Heuristics?
8. What is the data source?


# Streamlit App User Interface

## Page 1: Rainfall prediction
* User Interface with inputs (city and weather data) and prediction indicating the chance of raining tomorrow. If it is greater than 50%, it will rain. In that case, it tells, how much it would rain

## Page 2: Rainfall Seasonality
* User Interface with menu to select a city. 
* 3 Line chart plots indicating, respectively, the seasonality for Year, Month and Day.

## Page 3: WeatherClf
* Evaluation metrics/performance on WeatherClf
  * For both train and test set: Confusion Matrix and Classification Report
  * Bias/Variance Tradeoff

## Page 4: WeatherReg
* Evaluation metrics/performance on WeatherReg
  * For both train and test set: R2, RMSE, MSE, MAE
  * Bias/Variance Tradeoff

## Page 5: WeatherClust
* Evaluation metrics/performance on WeatherClust
  * 3D Scatter Plot for PCA with 3 components, colored by clusters
  * Silhouete score


# Django App User Interface

* It contains **Page 1** and **Page 2** from Streamlit App User Interface
