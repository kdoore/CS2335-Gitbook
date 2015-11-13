# Game Objects

###StateManager ObjectReferences
When each ``state`` object is initialized, it's constructor has an input parameter that is an object reference to the singleton instance of the StateManager object, this gets assignned to a local reference variable: `manager`. Later, we pass that address along to the next state object when it is time to change states by calling the constructor for the next state.  The SwitchState process is interesting because the ``SwitchState() method belongs to the StateManager object, but the code is executed from within the current state, but the function requires the current state to call the function by calling the constructor method for the next state.  This code really illustrates the fact that methods are used by objects to communicate, they act to allow messages to be sent between objects. 

The UnityEngine provides methods that allow easy message communication between objects, some of these methods are FindGameObject(), AttachListener(), OnTriggerEnter2D(), etc.
