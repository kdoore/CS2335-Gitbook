# Find GameObjects for use in Custom Scripts

###Learning Goals:
- Learn how to access current and interact with scene GameObjects and Components within custom class scripts.
- Learn how to use image files as Unity 2D sprites and UI elements.
- Learn how to create UI-Buttons from images, and to use those button to toggle visibility of UI-Panels.


###Object References
When we create object references, we're creating a variable that stores a memory address for an object.  For primitive data-types, variables are a labeled space in memory that stores data of the type that we specified when we defined the variable.  However, objects are more complex data types, the system can't know in advance how large of a space in memory we might need for our object, so instead the variable we create for an object holds a 'pointer' or address of the place in memory where our object will be stored.  This is very nice because it allows us to share the address, we can use it as in input variable for a function, then within the function, changes made actually do change the object itself, since we are referring directly to the object via it's address.  This is different than when we pass primitive types into a function, in that case, only a copy of the variable in actually passed in, this is part of the reason that variable scope is important for primitive data types.  Variable scope behaves somewhat  differently for object references, since we're passing the address into the function. 

```java
int someInteger;   //primitive type variable - stores integer in variable's memory space

someInteger=5;

public  GameObject mainPanel; // declaring a variable for objects of GameObject type.

mainPanel=GameObject.Find ("MainPanel1");  //we're trying to find the address of the GameObject, so we can interact with it by calling it's methods or changing it's properties.

```

###StateManager ObjectReferences
When each 'state' object is initialized, it's constructor takes an object reference to the singleton instance of the StateManager object and assigns that value to a local variable we've created to hold that address. Then later  we pass that address along when it changes states by calling the state constructor for the next state.  The change state process is quite interesting because the SwitchState() method belongs to the StateManager object, but the code is from within the current state, but the function requires the current state to call the function by calling the constructor method for the next state.  This code really illustrates the fact that methods are used by objects to communicate, they act to allow messages to be sent between objects. 
The UnityEngine provides methods that allow easy message communication between objects, some of these methods are FindGameObject(), AttachListener(), OnTriggerEnter2D(), etc.

```
manager.SwitchState(new BeginState(manager));
```

###Errors - Reference not found
Since we are creating references ( connections ) to GameObjects in our scenes from within our *State* classes, it seems that there can be some problems if the scene initialization does not occur before the new state object is initialized and begins executing methods that are called from the StateManager.  It's important to think about exactly what is causing these problems and how to fix them.  So, we have 2 independent entities:  a game scene that has GameObjects, an instance of our activeState state object.  The StateManager is responsible for managing the communication between our *state object*, and since it is attached to the GameManager GameObject, it is also able to execute Unity events like Awake, Update,  


It seems that finding Button GameObjects and Button Components causes the most trouble when initializing GameObject references.  I think that by wrapping the initialization of these references in an if statement where you check to verify the GameObject Button has been found will resolve the problem. The first part of the if() test is to make sure this code is only exectued one time since we don't want to keep wasting processing resources. By testing whether the startButton reference has been initialized, or is still null...as the first test, then once the inner initialization code has been executed once, this test will fail, so we no longer even look to see if we can find the "BackToStart" game object, as that is the resource intensive test case. 

See the code below:

```java
//State1.cs
    
    private bool initialized=false;
	private bool sb=false;

 [RuntimeInitializeOnLoadMethod] //are invoked after scene has been loaded
	void initializeObjectRefs(){ 
	    //find the MainPanel Canvas Group: doesn't seem to cause problems
		mainPanel=GameObject.Find ("MainPanel1");
		Debug.Log ("Found Main Panel in initialize Refs");
		
		//Find buttons and button components: 
		if((startButton == null)  && GameObject.Find("BackToStart") != null){ 
			startButton=GameObject.Find("BackToStart");
			Debug.Log ("found startButton")	;
			startBtnComponent=startButton.GetComponent<Button>();
			Debug.Log ("found start button component");
			startBtnComponent.onClick.AddListener(GoToStartScene);
			Debug.Log ("start button added listener:  ");
			sb=true;
		  }  
		  //no Button GameObject with <Button>component is active
		
		}
		
	//StateUpdate is executed each update() 
	//it's called from StateManager on the activeState
	public void StateUpdate(){  
	    
	    if(!initialized){  //if it returns false, if it returns true, don't run it again
	        initializeObjectRefs();  //returns false when it fails
	        if(sb){  //start button has been initialized (add more tests in here if you have more buttons, all flags need to be true:  if(sb && sb2 && sb3)
	           initialized=true;
	        }
	    }	
	}
	
	```
	
	


