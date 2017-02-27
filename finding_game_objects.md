# Finding Game Objects


###Learning Goals:
- Learn how to access current and interact with scene GameObjects and Components within custom class scripts.
- Learn how to use image files as Unity 2D sprites and UI elements.
- Learn how to create UI-Buttons from images, and to use those button to toggle visibility of UI-Panels.

###Object References: 
When we create object references, we're creating a variable that stores a memory address for an object.  For primitive data-types, variables are a labeled space in memory that stores data of the type that we specified when we defined the variable.  However, objects are more complex data types, the system can't know in advance how large of a space in memory we might need for our object, so instead the variable we create for an object holds a ``pointer`` or address of the place in memory where our object will be stored.  This is very nice because it allows us to share the address, we can use it as in input variable for a function, then within the function, changes made actually do change the object itself, since we are referring directly to the object via it's address.  This is different than when we pass primitive types into a function, in that case, only a copy of the variable in actually passed in, this is part of the reason that variable scope is important for primitive data types.  Variable scope behaves somewhat  differently for object references, since we're passing the address

   ```
int someInteger;   //primitive type variable stores integer in variable's memory space
    someInteger=5;  //assign a value

    public  GameObject mainPanel; // declaring a variable for objects of GameObject type.

    mainPanel=GameObject.Find ("MainPanel1");  //assign address of gameObject to referenceVariable so we can interact with it throughout the state class code

   ```
###StateManager ObjectReferences
The SwitchState process is quite interesting because the `SwitchState()` method belongs to the StateManager object, but the code is executed from within the current state, but the function requires the current state to call the function by calling the constructor method for the next state.  This code really illustrates the fact that methods are used by objects to communicate, they act to allow messages to be sent between objects. 
The UnityEngine provides methods that allow easy message communication between objects, some of these methods are FindGameObject(), AttachListener(), OnTriggerEnter2D(), etc. [Unity GameObject.Find Documentation](http://docs.unity3d.com/ScriptReference/GameObject.Find.html)

Code in BeginState to switch state and scene to correspond to the EndState, EndScene.  To do that, first we are assuming that the LoadEndScene( ) method will be activated when the endButton is clicked.  Once that happens, we use the Unity SceneManagement's SceneManager static method: LoadScene( "EndScene" ) , here we pass in the name of the Unity Scene that we want to go to.  Next, we use the singleton instance for the StateManager so we can call StateManager method: SwitchState().  When calling SwitchState, we first call the constructor for the new state: new EndState( ), and this object is passed as an input parameter for SwitchState( ), this is how StateManager is able to update the value of activeState, so it will now point to the newly created state.
These 2 actions must be performed each time we want to leave 1 scene and go to another scene.

```java

public void LoadEndScene ()
	{  
		Debug.Log ("Add Debug Info");
		SceneManager.LoadScene ("EndScene");  //actual scene name
		StateManager.instanceRef.SwitchState (new EndState ());  //create new state, pass to StateManager
	}
```
###Errors - Reference not found
Since we are creating references ( connections ) to GameObjects in our scenes from within our *State* classes, it seems that there can be some problems if the scene initialization does not occur before the new state object is initialized and begins executing methods that are called from the StateManager. 


   


