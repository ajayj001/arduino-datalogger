# Requirements Specifications and Viability Analysis

## Purpose and system overview
The purpose is to design an __Autonomous system for sensor data logging based on Arduino__. A prototype of the system is to be implemented.

The main function of the system is to record data from one/several sensors of any kind, and to save the logged data into a memory card. In order to have an autonomous system, the device will be equiped with a battery for power supply. Additionally, the system will run standalone, by including a LCD and a keypad.

### Initial specification
The designed system must comply with the following specifications.

* Sensor data will be locally stored in a memory card.
* Firmware will be designed in a generic fashion, emphasizing simplicity in new sensor addition.
* Sensors may have different sampling frequencies.
* Data will be stored using a suitable format, allowing for different sampling frequencies. JSON is proposed.
* GPS module will not be considered a sensor, but part of the system.
* GPS module will be used to obtain date/time, position (lat-lng coordinates) and velocity of the system.
* A LCD display will be used to show the status of the system: idle/working, remaining battery, possible errors (SD full, etc.).
* The first prototype will count with one (or more) pressure sensor(s).
* A computer application will provide a friendly interface to data recorded in the memory card.
* The computer application will count with a map-based user interface, on which the registered path will be represented.
* By clicking the different points on the path, sensor values at these points will be displayed.

### Proposed hardware
The following hardware is proposed to build the first prototype.

#### Arduino UNO
![Arduino UNO](img/arduino-uno.png)

#### Adafruit Ultimate GPS Logger Shield
This shield provides the _Device_ with GPS interface and a SD socket.
![Adafruit Ultimate GPS Logger Shield](img/adafruit-ultimate-gps-logger-shield.png)

#### Adafruit PowerBoost 500 Shield
The device will be powered by a 3.7v 2500mAh Lithium Ion Polymer Battery, attached to this shield.
![Adafruit PowerBoost 500 Shield](img/adafruit-powerboost-500-shield.png)

#### Adafruit RGB LCD Shield
This shield consists of a 16x2 Character LCD and a 5-button keypad. It will provided the _Human-machine interface_ between the _Device_ and the _User_.
![Adafruit RGB LCD Shield](img/adafruit-rgb-lcd-shield.png)

#### Pressure sensor shield



## Requirements specifications

### Definitions
* **Device**: Arduino-based datalogger (aka the system)
* **App**: computer application to work with recorded files (aka computer application)
* **User**: the person using the _Device_ or the _App_
* **Human-machine interface (HMI)**: interface between the _User_ and the _Device_
* **Graphical user interface (GUI)**: interface between the _User_ and the _App_
* **Log file**: file generated after a recording session (aka record file). A log file consists of multiple _Record_ s.
* **Record**: a piece of logged data, consisting of a timestamp, position (latitude and longitude coordinates), velocity and sensor values.
* **System parameters**: variables in the device used to change its behavior (e.g. sampling frequency).
* **Configuration file**: a special file stored in the memory card containing parameters specified by the user.

### Actors involved
#### Device
It is composed of the following hardware:

* Batterry Module
* Display
* Keypad
* Memory Card
* GPS Module
* Sensor Module(s)

#### User

### Requirements analysis

#### Functional requirements
* **FR-1**: Datalogging. The _Device_ logs data from several sensors.
	* **FR-1.1**: Sensor data will be locally stored in a memory card.
	* **FR-1.2**: Sensors are associated to particular sampling frequencies.
	* **FR-1.3**: These frequencies can be changed by the user.
	* **FR-1.4**: Every single record will be composed of a timestamp, position (latitude-longitude coordinates), velocity and sensor values.
	* **FR-1.5**: Only values from "measured sensors" will be present on each record. "Measured sensors" depends on sensor frequency.
	* **FR-1.6**: Records are stored in the memory card in the form of log files.
	* **FR-1.6**: The first prototype reads pressure values from a pressure sensor.
* **FR-2**: Power supply. The main device's power supply is a battery.
* **FR-3**: Human-machine interface. The device can be controlled by the user standalone.
	* **FR-3.1**: The device works standalone.
	* **FR-3.2**: The device informs about its status using a LCD.
	* **FR-3.3**: The device takes user inputs from a keypad.
* **FR-4**: Computer application. A computer application allows the user to visualize and interact with logged data.
	* **FR-4.1**: The app can load log files.
	* **FR-4.2**: The app contains a map.
	* **FR-4.3**: The path in loaded files are shown on the map.
	* **FR-4.4**: Clicking a point on the path informs the user about the record taken on that point.
* **FR-5**: Configuration. Some parameters are stored in a configuration file.
	* **FR-5.1**: System parameters are saved into the memory in the form of a configuration file.
	* **FR-5.2**: The device can load and parse the configuration file.
	* **FR-5.3**: The device loads the configuration file on startup, adjusting system parameters.
	* **FR-5.4**: The device provides an interface to change the system parameters.

#### Non-functional requirements
##### Performance
* **NFR-1**: The maximum allowed sampling frequency is to be maximized.
* **NFR-2**: The capacity of the memory card must be enough to ensure a 30-minute recording (at least).
* **NFR-3**: The format for the records must minimize the size of the log, thus optimizing the available memory.
* **NFR-4**: The format for the records must allow to save a different number of sensors on every single record.
* **NFR-5**: Power consumption must be minimized.
* **NFR-6**: The app can load any number of log files.

##### Interoperability
* **NFR-5**: The log file format must be easily read by the device and the app.
* **NFR-8**: The log file format must be a well documented standard.
* **NFR-9**: The configuration file format must be a well documented standard.
* **NFR-10**: The app must run on a standard MS Windows PC with minimal setup.

##### Stability
* **NFR-11**: The device can be turned off without risk for current or previous records.

##### Reusability
* **NFR-12**: Adding more sensors to de device must be simple.


### Use cases
The following is a use case diagram, showing the user's interaction with the system. Similarly, this use case diagrams represents the hardware modules' interaction.

![USE CASE DIAGRAM][use-case-diagram]

#### User

##### Turn on the Device
- __Primary actor__: User
- __Scope__: Device interaction
- __Brief__: The _Device_ is turned on
- __Preconditions__:
	* The _Device_ is off
- __Postconditions__:
	* The _Device_ is on, starting the _Boot sequence_
- __Basic flow__:
	1. The _User_ flips the _Power switch_
	2. The _Device_ automatically turns on
	3. The _Boot sequence_ is automatically started
	4. The system puts itself into _Display ON state_
	5. A "Wellcome message" is displayed for a short time
	6. The current status is displayed

##### Turn off the Device
- __Primary actor__: User
- __Scope__: Device interaction
- __Brief__: The _Device_ is turned off
- __Preconditions__:
	* The _Device_ is on
- __Postconditions__:
	* The _Device_ is safely turned off
- __Basic flow__:
	1. The _User_ makes sure the device is in _Idle state_
	1. The _User_ flips the _Power switch_
- __Extensions__:
	* [1].
		1. The _User_ flips the _Power switch_ (THIS IS NOT SAFE)


##### Start data recording
- __Primary actor__: User
- __Scope__: Device interaction
- __Brief__: The _User_ puts the _Device_ into _Recording state_.
- __Preconditions__:
	* All the sensors are correctly initialized
	* GPS information is available (both time and position)
	* The _Device_ is in _Idle state_ (previous data successfully recorded)
	* There is free memory enough for a 5-minutes record (the _Device_ will warn the _User_ on low free memory)
- __Postconditions__:
	* The _Device_ is in _Recording state_
	* The _GPS Module_ and every _Sensor Module_ record data, which is stored in the _Memory Card_
- __Basic flow__:
	1. The system checks the preconditions
	2. The system puts itself in _Recording state_
	3. When the _User_ requires it, the system stops recording, going back to _Idle state_
- __Extensions__:
	* [2].
		1. On sensor fault, the system keeps on recording, inserting *null* on that field. This applies for GPS disconnection
		2. On memory card fault, the system reinitializes the _Memory Module_, keeping on recording
	* [3].
		1. On low memory, the system safely stops recording, putting itself into _Idle state_

##### Stop data recording
- __Primary actor__: User
- __Scope__: Device interaction
- __Brief__: The _User_ puts the _Device_ back into _Idle state_.
- __Preconditions__:
	* The _Device_ is in _Recording state_
- __Postconditions__:
	* The _Device_ is in _Indle state_
- __Basic flow__:
	1. The memory buffer is written to the memory
	2. The system puts itself in _Idle state_

##### Check status
- __Primary actor__: User
- __Scope__: Device interaction
- __Brief__: When the _Display_ is turned on, the current status of the _Device_ is displayed
- __Preconditions__:
	* The _Device_ is in _Display ON state_
- __Postconditions__:
	* The current status of the _Device_ is displayed
- __Basic flow__:
	1. The _Display_ shows the _Device status_ until it is turned off
- __Extensions__:
	* [1].
		1. From _Display ON state_, the _User_ can get into the menu by pressing a specific button on the _Keypad_, thus entering the _Menu_. Some elements of the _Device status_ might not be displayed when in the _Menu_.

###### Date and time
- __Brief__: The date and time are displayed when the status is checked

###### Remaining battery
- __Brief__: Remaining battery is displayed when the status is checked

###### GPS connected
- __Brief__: It is displayed whether the GPS is connected when the status is checked

###### Current state
- __Brief__: The current state (e.g. _Idle state_) is displayed when the status is checked, either directly or indirectly

##### Adjust recording options
- __Primary actor__: User
- __Scope__: Device interaction
- __Brief__: A menu is available, allowing the user to adjust some recording options
- __Preconditions__:
	* The _Device_ is in _Display ON state_
- __Postconditions__:
	* The recording options are changed
	* The new recording options are immediately stored in the _Memory Card_
- __Basic flow__:
	1. The _User_ enters the _Menu_ by pressing a specific button on the _Keypad_
	2. The _User_ selects options from this menu
	3. The _User_ changes the values for these options
	4. The _User_ exits the _Menu_
	5. The new values for the options are stored in the _Memory Card_
	6. The _Config file_ is loaded from the _Memory Card_
- __Extensions__:
	* [3-6].
		1. If the _User_ does not change the recording options, the new values are not stored and the _Config file_ is not loaded


#### Device

##### Init (initialization sequence)
- __Primary actor__: Device
- __Scope__: Initialization
- __Brief__: The _Device_ initializes every hardware module. The scheduler and other software elements are initialized. The system reads the configuration file.
- __Preconditions__:
	* The _Device_ is turned on
- __Postconditions__:
	* The _Device_ is correctly initialized
	* Every hardware module is initialized
	* The scheduler is loaded and tasks start running on a cyclical fashion
	* The configuration is loaded
- __Basic flow__:
	1. The _Device_ is turned on
	2. The hardware modules are initialized
	2. The task scheduler is initialized
	4. The configuration is loaded from the memory card
	5. The tasks start running
	6. The system informs the user about the current status
- __Extensions__:
	* [4].
		1. The configuration file is missing
		2. A default configuration file is created

##### Get battery status
- __Primary actor__: Device
- __Scope__: Read the status of the Device
- __Brief__: The _Device_ gets knowledge of the battery status
- __Preconditions__:
	* The _Device_ is on
- __Postconditions__:
	* The _Device_ knows the status of the battery
	* The _Device_ knows whether the battery is being charged
- __Basic flow__:
	1. A certain timeout occurs
	2. The _Device_ asks the _Battery Module_ for battery information
	3. The _Device_ gets the required information
	4. The timeout is reseted
- __Extensions__:
	* [3-4].
		1. The _Device_ is not able to get the required information
		2. The _Device_ signals an error on the battery module
		3. The timeout is reseted

##### Turn on the display
- __Primary actor__: Device
- __Scope__: Device interaction
- __Brief__: The _Display_ is turned on
- __Preconditions__:
	* The _Device_ is in _Display OFF state_
- __Postconditions__:
	* The _Device_ is in _Display ON state_, then back again to _Display OFF state_
- __Basic flow__:
	1. The _User_ presses any button on the _Keypad_
	2. The _Device_ puts itself into _Display ON state_
	3. A certain timer is set to a certain value
	4. The _Display_ shows relevant information for several seconds, allowing the _User_ to enter the _Menu_
	5. On timeout, the _Device_ puts itself back to _Display OFF state_, turning off the display in order to save energy
- __Extensions__:
	* [5].
		1. Any button on the _Keypad_ is pressed
		2. The timer is reset to the value specified in [3], thus increasing the time the _Display_ is on

##### Display status
- __Primary actor__: Device
- __Scope__: Device interaction
- __Brief__: The current status of the _Device_ is displayed
- __Preconditions__:
	* The _Device_ is in _Display ON state_
- __Postconditions__:
	* The current status of the _Device_ is displayed
- __Basic flow__:
	1. The _Device_ enters _Display ON state_
	2. The current status of the _Device_ is displayed
- __Extensions__:
	* [2].
		1. A certain button on the _Keypad_ is pressed
		2. The _Menu_ is displayed (see *Display menu*)

##### Display menu
- __Primary actor__: Device
- __Scope__: Device interaction
- __Brief__: The _Menu_ is displayed. When the user navigates through it, the different options and values are displayed.
- __Preconditions__:
	* The _Device_ is in _Display ON state_
- __Postconditions__:
	* The _Menu_ is conviniently displayed
	* Some indications to navigate it are given to de user
	* When the user navigates through the menu, the view is refreshed
- __Basic flow__:
	1. The _Device_ is in _Display ON state_
	2. A certain button on the keypad is pressed
	3. The main view of the _Menu_ is displayed
	4. The user is allowed to navigate the menu
	5. Any button is pressed
	6. The view of the _Menu_ is updated

- __Extensions__:
	* [5-6].
		1. A certain button on the _Keypad_ is pressed
		2. The menu is closed, thus the status is displayed (see *Display status*)


##### Register user actions
- __Primary actor__: Device
- __Scope__: Device interaction
- __Brief__: The _User_'s actions are conviniently registered using the _Keypad_
- __Preconditions__:
	* The _Device_ is on
- __Postconditions__:
	* The system registers every keypress on real time
	* The actions always produce a change on the display
	* If the _Device_ is in _Sleep mode_, it is waken up by the keypress
- __Basic flow__:
	1. A button on the keypad is pressed
	2. An action is registered
	3. The system makes some changes, according to the action
	4. The new status is displayed

- __Extensions__:
	* [4].
		1. If the status has not changed, some signal will be displayed to let the user know

##### Save data
- __Primary actor__: Device
- __Scope__: Data storage
- __Brief__: The data is conviniently and safely stored in the _Memory Card_
- __Preconditions__:
	* The _Device_ is in _Recording state_
- __Postconditions__:
	* The data is stored safely int he _Memory Card_
- __Basic flow__:
	1. A certain condition related to the buffer and the time occurs ( **TBD** )
	2. The memory buffer is written to the memory

##### Get available memory
- __Primary actor__: Device
- __Scope__: Data storage
- __Brief__: The available memory is continously checked in order to inform the user about it and to prevent erros
- __Preconditions__:
	* The _Device_ is in _Recording state_
	* (OR) The _Device_ is in _Display ON state_
- __Postconditions__:
	* The system gets knowledge about the remaining available memory
- __Basic flow__:
	1. The memory buffer is written to the memory
	2. The available memory is checked
	3. On low memory, warn the user
	4. On very low memory, stop recording
- __Extensions__:
	* [1].
		1. The _Device_ enters _Display ON state_

##### Get position
- __Primary actor__: Device
- __Scope__: Data retrieval
- __Brief__: The device gets its position from GPS everytime a measure is taken
- __Preconditions__:
	* The _Device_ is in _Recording state_
- __Postconditions__:
	* The system gets knowledge about its current position
- __Basic flow__:
	1. Data is retrieved from other sensors
	2. The _Device_ gets its current position
- __Extensions__:
	* [2].
		1. The _Device_ cannot get its current position

##### Get time
- __Primary actor__: Device
- __Scope__: Data retrieval
- __Brief__: The device synchronizes its own RTC with GPS time
- __Preconditions__:
	* The _Device_ is on
- __Postconditions__:
	* The system synchronizes its own RTC with GPS time
- __Basic flow__:
	1. A certain timeout occurs
	2. The RTC is set to GPS time
- __Extensions__:
	* [1].
		1. On start up

##### Get data from sensor
- __Primary actor__: Device
- __Scope__: Data retrieval
- __Brief__: The device gets data from a _Sensor Module_
- __Preconditions__:
	* The _Device_ is in _Recording state_
- __Postconditions__:
	* The system gets knowledge of a sensed variable
- __Basic flow__:
	1. A certain timeout occurs
	2. The system reads the sensor value
- __Extensions__:
	* [2].
		1. The system cannot read the sensor value

##### Record data
- __Primary actor__: Device
- __Scope__: Data retrieval
- __Brief__: The device gets data from Sensor Modules and sends it to memory buffer, along with time and position. This is done periodically with a certain frequency, which is configurable. Every sensor has a configurable option (N) to specify "this sensor is read every N periods".
- __Preconditions__:
	* The _Device_ is in _Recording state_
- __Postconditions__:
	* The memory buffer is supplied with data from sensors, along with time-position information
- __Basic flow__:
	1. A certain timeout occurs
	2. The system gets data from sensors
	3. The system gets time and position
	4. The collected data is put together in the memory buffer
- __Extensions__:
	* [4].
		1. The memory buffer is full
		2. An error is reported to the user


### Machine state diagram
The following diagrams represent the main states the system will be on during execution.

![STATE MACHINE DIAGRAM: OVERALL][state-machine-overall]

![STATE MACHINE DIAGRAM: DISPLAY][state-machine-display]

![STATE MACHINE DIAGRAM: MENU][state-machine-menu]

### Class diagram
The following is the class diagram proposed for the system.

![CLASS DIAGRAM][class-diagram]

## Viability analysis
The requirements specified above seem to be achievable with the proposed hardware. Nevertheless, the following limitations have been detected and should be considered.

### System limitations
* The GPS module is not guaranteed to work properly at high altitudes. This should not be a problem, but errors could happen up on the mountain.
* The maximum sampling frequency imposed by the GPS module is 10Hz.
* Arduino UNO implements the ATmega328 microcontrooler, which counts with 32KB of Flash memory and 2KB of SRAM. Although this should be enough for the current application, it could limit the memory buffer, thus interfering with maximum sampling frequency. Needless to say, the limited memory amount limits reusability, since new features will always require extra memory.

### Power consumption analysis
Power consumption analysis is beyond the scope of this document.

### Proposed libraries
To achieve the specified architecture and functionality, the following libraries are proposed:

* [Task Scheduler](https://github.com/arkhipenko/TaskScheduler)
	* Provides multitasking capabilities
* [ardubson](https://github.com/argandas/ardubson)
	* Provides a convinient interface to BSON format
* [Adafruit GPS](https://github.com/adafruit/Adafruit_GPS)
	* Interfaces the GPS module and provides parsing capabilities
* [Adafruit RGB LCD](https://github.com/adafruit/Adafruit-RGB-LCD-Shield-Library)
	* Interfaces the LCD and Keypad

[use-case-diagram]: use-case.png
[state-machine-overall]: state-machine_overall.png
[state-machine-display]: state-machine_display.png
[state-machine-menu]: state-machine_menu.png
[class-diagram]: class-diagram.png
