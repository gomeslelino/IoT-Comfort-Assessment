# IoT-Comfort-Assessment
This Project combines IoT sensor principles with Data Science and Analytics through Machine Learning.

Sensors measuring  temperature,  humidity,  loudness  and  dust  particles were installed in a TUM lecture room, as well as a voting box asking users of the room if they feel comfortable or not. Data about the sensor readings and the public comfort perception were collected for one month (mid July to mid August 2023). A  random forest regressor  model was then used on the resulting database in order to determine the optimal conditions for comfort, as well as which factors influence people's perception of comfort the most.

<p align="center">
  <img width="460" height="300" src="https://github.com/gomeslelino/IoT-Comfort-Assessment/blob/main/Pictures/project%20goal.png">
</p>

The schema below illustrates the whole system setup:

<p align="center">
  <img width="400" height="206" src="https://github.com/gomeslelino/IoT-Comfort-Assessment/blob/main/Pictures/system%20parts.png">
</p>

## IoT Setup

The system was divided in two separate units and installed in the TUM lecture room. The first unit corresponds to the sensor setup, including temperature, humidity, loudness and dust sensors. The readings collected by the sensors would be sent through the LoRaWAN network and collected in a database every minute.

<p align="center">
  <img width="660" height="300" src="https://github.com/gomeslelino/IoT-Comfort-Assessment/blob/main/Pictures/sensor%20setup.png">
</p>


The second unit corresponded to the buttons setup. The users of the room could simply press either a "Yes Button" or a "No Button" in response to the question "Do you feel comfortable in this room right now?". The votes were collected in similar fashion anda lso sent through the LoRaWAN network, but with a lower frequency of every 5 minutes.

<p align="center">
  <img width="660" height="300" src="https://github.com/gomeslelino/IoT-Comfort-Assessment/blob/main/Pictures/Button%20setup.png">
</p>


## Data Collection

The data was collected and graphs were constantly updated presenting the readings in dashboards through Grafana:

<p align="center">
  <img width="660" height="300" src="https://github.com/gomeslelino/IoT-Comfort-Assessment/blob/main/Pictures/grafana.png">
</p>

Because the sensor unit and the button units were separated in the room, the data collection wasn't automatically synchronized. The month long readings for both units were extracted as .csv files, an synchronized through a short python script, so the readings could be compared to the button votes.

<p align="center">
  <img width="660" height="300" src="https://github.com/gomeslelino/IoT-Comfort-Assessment/blob/main/Pictures/synchronize%20data.png">
</p>

## Data Preprocessing

The excel file "IoTReadings" with the readings and button votes compiled was loaded into a Jupyter Notebook file for preprocessing. These were the steps for this phase:

1 - Blank Values Handling: somethings the sensors would fail to send a reading for one of the specific measured elements in short intervals. KNNImputer was used do handle this cases for the readins of Humidity, Temperature, dB and particles.

2 - Redistributing Votes: because the votes were uploaded to the cloud less frequently (every 5 minutes instead of 1), a script was necessary to redistribute the vote quantities for the minutes without uploads.

3 - Comfort Index: a new Feature called "Comfort Index" was calculated for each row of the data based on the amount of Yes and No votes, this column would later be used as the target column for the Machine Learning algorithm.

## Model Selection, Training and Evaluation

With the dataset preprocessed, we moved on to the Model Selection Phase. The regression models tested were "Linear Regression", "Decision Tree Regressor", "Random Forest Regressor", "Gradient Bosoting Regressor", "KNN Regressor" and "Support Vector Regressor". The following results were obtained, which highlighted the Random Forest Regressor as the most viable model for having the lowest NMSE and MAE and the highest R^2.

> Model: Linear Regression
> 
> Negative Mean Squared Error (NMSE): 0.17568305062773568
> 
> Mean Absolute Error (MAE): 0.35803671531489906
> 
> R-squared (R^2): -0.010611699529072963


> Model: Decision Tree Regressor
> 
> Negative Mean Squared Error (NMSE): 0.14559568728959046
> 
> Mean Absolute Error (MAE): 0.2084628968052881
> 
> R-squared (R^2): 0.07670026574252325


> Model: Random Forest Regressor
> 
> Negative Mean Squared Error (NMSE): 0.082522821052955
> 
> Mean Absolute Error (MAE): 0.21779745989018054
> 
> R-squared (R^2): 0.5265889310878603


> Model: Gradient Boosting Regressor
>
> Negative Mean Squared Error (NMSE): 0.10634273464772485
>
> Mean Absolute Error (MAE): 0.2559633047703341
> 
> R-squared (R^2): 0.3765981229352667


> Model: KNN Regressor
>
> Negative Mean Squared Error (NMSE): 0.10894697011577875
>
> Mean Absolute Error (MAE): 0.24982155693815775
>
> R-squared (R^2): 0.3715930716113586


> Model: Support Vector Regressor
>
> Negative Mean Squared Error (NMSE): 0.1366993051380376
>
> Mean Absolute Error (MAE): 0.2755233929820288
>
> R-squared (R^2): 0.21416503389236322


Then, I moved on to hyperparameter tuning for the Random Forest Regressor, obtaining the following result:

> Best Parameters: {'max_depth': 20, 'min_samples_split': 2, 'n_estimators': 300}

With the hyperparameters tuned, the best random forest regressors results were the following:

> Mean Squared Error (MSE): 0.0740180301403758
>
> Mean Absolute Error (MAE): 0.19353280802664863
>
> R-squared (R^2): 0.5813938547640598


## Feature Importance

From the feature importance analysis, it is important to conclude that Temperature impacts people's perception of comfort the most, followed by Humidity, Loudness, and in last place particles concentration.

<p align="center">
  <img width="660" height="400" src="https://github.com/gomeslelino/IoT-Comfort-Assessment/blob/main/Pictures/feature%20importance.png">
</p>

The correlation matrix generated with pearson method with seaborn showcases a similar result.

<p align="center">
  <img width="660" height="600" src="https://github.com/gomeslelino/IoT-Comfort-Assessment/blob/main/Pictures/heatmap.png">
</p>


## Identifying Optimal Comfort Conditions

With the model created, ranges were defined for each feature, and a python script would combine all the possible readings from these ranges to define which combination of readings would return the highest Comfort Index, or highest degree of comfort. A total of more than 160.000 combinations of input values for the features were tested, and the optimal room conditions for comfort are as follows:

> Optimal Temperature: 20.0
>
> Optimal Humidity: 30.0
>
> Optimal dB Value: 40.0
>
> Optimal ug/m3 (10) Value: 0.0

## Discussion

With this model, we are able to predict what the public's comfort perception will be when we have specific Temperature, Humidity, Loudness and Particles Concentration values as input, and we were also able to identify the perfect room conditions, an interesting tool if we want lecture rooms to be optimized for comfort, facilitating learning.
