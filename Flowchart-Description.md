
# DESCRIPTION OF THE ALGORITHM
## Variables
* **TempReal**  Real temperature outdoor
* **HumReal**   Real humidity outdoor
* **TempMax** 	Maximum temperature set indoor
* **TempMin** 	Minimum temperature set indoor
* **HumMax**  	Maximum humidity set indoor
* **HumMin**	  Minimum humidity set indoor
* **RoomDim** 	Room dimensions
* **Distance**	Recommended interpersonal distance
* **RTemp** 		Temperature sensor in the room 
* **RHum**  		Humidity sensor in the room
* **CriticalMaxHum, CriticalMaxHum**	          Critical humidity values indoors maximum and minimum respectively
* **Ventilation, Humidifier, dehumidifier, Heating**  	actuator states in the room (ON/OFF)
* **Clock**     Time during which the dehumidifier is ON, critical value 2 hours.
* **EC**	      Estimated Capacity of people with respect to humidity and room dimensions

## Events
* **Clock.reset** 	    set the clock in zero
* **Humidity alert 1**	_Humidity could not be reduced_ message
* **Humidity alert 2**	_The value of the humidity in the room is critical_ message
* **Increase distancing**	_the capacity of the establishment has decreased, please increase the distancing_ message

## Instructions
### 1. Get values of temperature and humidity obtained outdoors, and room dimensions.
Declare variables for maximum and minimum temperature and humidity ranges indoors
### 2. Set:
* Value of minimum humidity indoors 47%
* Minimum humidity critical value 40%
* Maximum humidity critical value 65%
### 3. Decide season, winter or summer based on temperature outdoors to set temperature and humidity ranges to keep indoors.
  1. If the temperature outdoors is higher or equal to 24º Celsius. Set:
* Maximum temperature 23º
* Minimum temperature 22º
* Maximum humidity 50%
  2. Else, the temperature is less than 24º. Set:
* Maximum temperature 26º
* Minimum temperature 24º
* Maximum humidity 55%
### 4. Get values of temperature and humidity indoors
5. If the census temperature is less than the minimum temperature
* the heating state changes to “ON”
else, if the value of the census temperature is greater than the maximum temperature
* the ventilation state changes to “ON”
otherwise
* the heating and ventilation states will be “OFF”.
6. If the census Humidity is less than the minimum humidity
* the value of the humidifier changes to “ON”
else, if the value of the census humidity is greater than the maximum humidity
* the value of the dehumidifier changes to “ON”
* the clock that indicates the operating time of this actuator is activated
* when the value of the clock exceeds 2  hours, the “Humidity Alert 1” is sent _this alert indicates that the humidity inside has not been able to be reduced_
otherwise
* the state of the humidifier and dehumidifier change to “OFF”
* the clock will be reset.
7. If the census Humidity is less than the minimum humidity critical value OR greater than the maximum humidity critical value
* the “Humidity Alert 2” and “Increase distancing” are sent _this indicates that the humidity value indoors is critical_, _the interpersonal distancing should be increased and the estimated capacity of people in the establishment has decreased.
* recommended distance is set to 2 meters.
Otherwise
* the recommended distance is set to 1.5 meters.
8. The estimated capacity calculated, is equal to the dimension of room in square meters, divided for the recommended distance plus 0.2 in meters.
9. The following message is printed
* _“The estimated capacity of people within the establishment is: “_ + **EC** (Estimated capacity)
