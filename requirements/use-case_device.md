### Init (initialization sequence)
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
	[4].
		1. The configuration file is missing
		2. A default configuration file is created

### Get battery status
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
	[3-4].
		1. The _Device_ is not able to get the required information
		2. The _Device_ signals an error on the battery module
		3. The timeout is reseted

### Turn on the display
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
	[5]. 
		1. Any button on the _Keypad_ is pressed
		2. The timer is reset to the value specified in [3], thus increasing the time the _Display_ is on

### Display status
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
	[2].
		1. A certain button on the _Keypad_ is pressed
		2. The _Menu_ is displayed (see *Display menu*)

### Display menu
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
	[5-6].
		1. A certain button on the _Keypad_ is pressed
		2. The menu is closed, thus the status is displayed (see *Display status*)


### Register user actions
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
	[4].
		1. If the status has not changed, some signal will be displayed to let the user know

### Save data
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

### Get available memory
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
	[1].
		1. The _Device_ enters _Display ON state_

### Get position
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
	[2].
		1. The _Device_ cannot get its current position

### Get time
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
	[1].
		1. On start up

### Get data from sensor
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
	[2].
		1. The system cannot read the sensor value

### Record data
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
	[4].
		1. The memory buffer is full
		2. An error is reported to the user


