# Find GameObjects for use in Custom Scripts

###Learning Goals:
- Learn how to access current and interact with scene GameObjects and Components within custom class scripts.
- Learn how to use image files as Unity 2D sprites and UI elements.
- Learn how to create UI-Buttons from images, and to use those button to toggle visibility of UI-Panels.

###Errors - Reference not found
Since we are creating references ( connections ) to GameObjects in our scenes from within our *State* classes, it seems that there can be some problems if the scene intialization does not occur before the new state object is initialized and begins executing methods that are called from the StateManager.  It's important to think about exactly what is causing these problems and how to fix them.  So, we have 2 independent entities:  a game scene that has GameObjects, an instance of our activeState state object.  The StateManager is responsible for managing the communication between our *state object*, and since it is attached to the GameManager GameObject, it is also able to execute Unity events like Awake, Update,  


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
	        if(sb){  //start button has been initialized
	           initialized=true;
	        }
	    }	
	}
	
	```