### Turn on the Device
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

### Turn off the Device
- __Primary actor__: User
- __Scope__: Device interaction
- __Brief__: The _Device_ is turned off
- __Preconditions__:
	* The _Device_ is on
- __Postconditions__:
	* The _Device_ is safely turned off
- __Basic flow__:
	**TBD**
- __Extensions__:
	1. The _User_ flips the _Power switch_ (THIS IS NOT SAFE)

### Turn on display
- __Primary actor__: User
- __Scope__: Device interaction
- __Brief__: The _Display_ is turned on
- __Preconditions__:
	* The _Device_ is in _Display OFF state_
- __Postconditions__:
	* The _Device_ is in _Display ON state_, then back again to _Display OFF state_
- __Basic flow__:
	1. The _User_ presses any button on the _Keypad_
	2. The _Device_ puts itself into _Display ON state_ 
	3. The _Display_ shows relevant information for several seconds, allowing the _User_ to enter the _Menu_
	4. On timeout, the _Device_ puts itself back to _Display OFF state_, turning off the display in order to save energy
- __Extensions__:
	[4]. On keypress, the timeout is restarted, thus increasing the time the _Display_ is on

### Start data recording
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
	[2].
		1. On sensor fault, the system keeps on recording, inserting *null* on that field. This applies for GPS disconnection
		2. On memory card fault, the system reinitializes the _Memory Module_, keeping on recording
	[3]. 
		1. On low memory, the system safely stops recording, putting itself into _Idle state_

### Stop data recording
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

### Check status
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
	[1].
		1. From _Display ON state_, the _User_ can get into the menu by pressing a specific button on the _Keypad_, thus entering the _Menu state_. Some elements of the _Device status_ might not be displayed in _Meny state_

#### Date and time
- __Brief__: The date and time are displayed when the status is checked

#### Remaining battery
- __Brief__: Remaining battery is displayed when the status is checked

#### GPS connected
- __Brief__: It is displayed whether the GPS is connected when the status is checked

#### Current state
- __Brief__: The current state (e.g. _Idle state_) is displayed when the status is checked, either directly or indirectly

### Adjust recording options
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
	[3-6].
		1. If the _User_ does not change the recording options, the new values are not stored and the _Config file_ is not loaded

