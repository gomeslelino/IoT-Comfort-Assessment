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

## Data Description
## Technical Overview
## Requirements
## Result
