* RainTomorrow - humidity3pm, cloud3pm, , sunshine
* Rainfallomorrow- 


|     Variable     | RowsWithMissingData | PercentageOfDataset | DataType |                        Method                        |
|:----------------:|:-------------------:|:-------------------:|:--------:|:----------------------------------------------------:|
|     Sunshine     |        69835        |        48.01        |  float64 |        drop variable high pps with humidty3pm        |
|    Evaporation   |        62790        |        43.17        |  float64 |      drop - high pps with temp3pm and max temp       |
|     Cloud3pm     |        59358        |        40.81        |  object  | CategoricalImputer -keep,hihg pps with rain tomorrow |
|     Cloud9am     |        55888        |        38.42        |  object  |         drop - high correlated with cloud3pm         |
|    Pressure9am   |        15065        |        10.36        |  float64 |                   Median Imputation                  |
|    Pressure3pm   |        15028        |        10.33        |  float64 |                   Median Imputation                  |
|    WindDir9am    |        10566        |         7.26        |  object  |                  CategoricalImputer                  |
|    WindGustDir   |        10326        |         7.1         |  object  |                  CategoricalImputer                  |
|   WindGustSpeed  |        10263        |         7.06        |  float64 |                   Median Imputation                  |
|    Humidity3pm   |         4507        |         3.1         |  float64 |                   Median Imputation                  |
|    WindDir3pm    |         4228        |         2.91        |  object  |                  CategoricalImputer                  |
|      Temp3pm     |         3609        |         2.48        |  float64 |                   Median Imputation                  |
|   RainTomorrow   |         3267        |         2.25        |  object  |                        drop na                       |
|   RainfallToday  |         3261        |         2.24        |  float64 |                        drop na                       |
| RainfallTomorrow |         3262        |         2.24        |  float64 |                        drop na                       |
|     RainToday    |         3261        |         2.24        |  object  |                        drop na                       |
|   WindSpeed3pm   |         3062        |         2.11        |  float64 |                   Median Imputation                  |
|    Humidity9am   |         2654        |         1.82        |  float64 |                   Median Imputation                  |
|      Temp9am     |         1767        |         1.21        |  float64 |                   Median Imputation                  |
|   WindSpeed9am   |         1767        |         1.21        |  float64 |                   Median Imputation                  |
|      MinTemp     |         1485        |         1.02        |  float64 |                         Mean                         |
|      MaxTemp     |         1261        |         0.87        |  float64 |                      Median Imputation               |
|                  |                     |                     |          |                                                      |
|                  |                     |                     |          |                                                      |
|                  |                     |                     |          |                                                      |
|                  |                     |                     |          |                                                      |



