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
When each 'state' object is initialized, it's constructor takes an object reference to the singleton instance of the StateManager object and assigns that value to a local variable: `manager`, we've created to hold that address. Later, we pass that address along to the next state object when it is time to change states by calling the constructor for the next state.  The SwitchState process is quite interesting because the `SwitchState()` method belongs to the StateManager object, but the code is executed from within the current state, but the function requires the current state to call the function by calling the constructor method for the next state.  This code really illustrates the fact that methods are used by objects to communicate, they act to allow messages to be sent between objects. 
The UnityEngine provides methods that allow easy message communication between objects, some of these methods are FindGameObject(), AttachListener(), OnTriggerEnter2D(), etc. [Unity GameObject.Find Documentation](http://docs.unity3d.com/ScriptReference/GameObject.Find.html)

```
manager.SwitchState(new BeginState(manager));
```
###Errors - Reference not found
Since we are creating references ( connections ) to GameObjects in our scenes from within our *State* classes, it seems that there can be some problems if the scene initialization does not occur before the new state object is initialized and begins executing methods that are called from the StateManager.  It's important to think about exactly what is causing these problems and how to fix them.  So, we have 2 independent entities:  a game scene that has GameObjects, and an instance of our activeState state object.  The StateManager is responsible for managing the communication between our *state object*, and since it is attached to the GameManager GameObject, it is also able to execute Unity events like Update, and OnGUI. 


It seems that finding Button GameObjects and Button Components causes the most trouble when initializing GameObject references.  I think that by wrapping the initialization of these references in an if statement where you check to verify the GameObject Button has been found will resolve the problem. The first part of the if() test is to make sure this code is only exectued one time since we don't want to keep wasting processing resources. By testing whether the mainPanel reference has been initialized, or is still null...as the first test, then once the inner initialization code has been executed once, this test will fail, so we no longer even look to see if we can find the GameObject.Find("MainPanel") which is the resource intensive test case.  ``if(mainpanel==null && GameObject.Find ("MainPanel1") !=null)``

See the code below:

```
//State1.cs
    
    private bool initialized=false;
	private bool sb=false, mp=false;  //object reference flags to incicate the references have been successfully initialized

	void initializeObjectRefs(){ 
	    //find the MainPanel Canvas Group: doesn't seem to cause problems
		if(mainpanel==null && GameObject.Find ("MainPanel1") !=null){
		    mainPanel=GameObject.Find ("MainPanel1");
		    Debug.Log ("Found Main Panel in initialize Refs");
		    mp=true;  //flag indicates mainpanel object reference has been initialized
		    }
		
		//Find buttons and button components - this seems to cause problems in some scenes
		if((startButton == null) && GameObject.Find("BackToStart") != null){ 
			startButton=GameObject.Find("BackToStart");
			Debug.Log ("found startButton")	;
			startBtnComponent=startButton.GetComponent<Button>();
			Debug.Log ("found start button component");
			startBtnComponent.onClick.AddListener(GoToStartScene);
			Debug.Log ("start button added listener:  ");
			sb=true;//flag indicates startButton obj ref has been initialized
		  }  
		  //no Button GameObject with <Button>component is active
		
		 if(sb && mp){  //start button has been initialized (add more tests in here if you have more buttons, all flags need to be true:  if(sb && sb2 && sb3)
	           initialized=true;
	        }
		}
		
	//StateUpdate is executed each update() 
	//it's called from StateManager on the activeState
	public void StateUpdate(){  
	    
	    if(!initialized){  //if it returns false, if it returns true, don't run it again
	        initializeObjectRefs();  //returns false when it fails
	       
	    }	
	}
```	

###Crashing Unity
For the code above, you need to make sure that you're not calling initializeObjectRefs(); more than a couple times, otherwise, the constant attempts to find gameObjects will crash your system.  Make sure to look at your logic that determines if initializeObjectRefs() gets called.  The ordering of your test cases matters! You want to first test the condition that is not checking for GameObject.Find(""), so in the starter code above, notice that the first thing to check is whether the gameObject reference: mainpanel==null  If we were testing GameObject.Find("MainPanel"), first, then we'd look through every GameObject every update() loop, that can cause lag in our system.    Using individual flags for each gameOjbect: sb and mp, insure that each GameObject is initialized, otherwise we'll keep calling InitializeObjectRefs() until all flags have been set to true.  It may be that you only have to wrap one gameObject in this protective wrapper, in that case you're only checking 1 flag in order to set initialized=true;

	``` 
	//inside initializeObjectRefs, check for each flag to be true
	    if(sb ){  //start button has been initialized
	           initialized=true;
	        }
		
	```
	

###Text Input
How to script an input Box:  This code is in one of the State.cs files where we are trying to capture the input from an InputText GameObject on the corresponding Scene.  The difficult part of this code is determining how to capture the event when the user has submitted the input by hitting the enter key.  We need to determine how we can add an event Listener within our State.cs instance.

```
	   
	  // State.cs instance variable
      private InputField playerName;
      
      
      public void InitializeObjectRefs (){
        playerName = GameObject.Find ("PlayerName").GetComponent<InputField> ();
		var se=new InputField.SubmitEvent();   //dynamic variable type //let unity figure out the datatype
		se.AddListener(ProcessInput);
		playerName.onEndEdit=se;
      }
```	


this sets up a call to this created function, the input is a string :  This assumes that in ``StateManager`` we have declared a string public field or property: ``public String playerName``
```
    public void ProcessInput(string arg0){
		Debug.Log (arg0);
		managerRef.playerName=arg0;
		Debug.Log ("Player Name " + managerRef.playerName);
	}
    ```




