# IStateBase
IStateBase is a custom interface that we will have all StateX classes implement.  This guarantees that all States will implement all Properties and Methods defined in this interface. 

StateManager,  will use an IStateBase object reference to keep track of the current active state instance.  StateManager will execute the methods  defined the interface IStateBase for the currently activeState.  StateManager delegates responsibility to the current activeState using these methods.

IStateBase 

```
// in StateManager

IStateBase activeState;  //

void Start(){
    activeState = new BeginState( this );  //reference to first activeState
}

```

```
using UnityEngine;
using System.Collections;

/// <summary>
/// I state base.
/// Interface for all StateX.cs classes
/// </summary>
public interface IStateBase{
		
	gameState State // Inteface Property
	{
		get;
	}    

	//all interface methods are public by default!

	void StateUpdate (); //executed by StateManager in Update(), executed once per frame

	void StateGUI (); // can execute any onGUI() code

	void InitializeObjectRefs ();  //cache object References once Scene is loaded
}


```

###StateX.cs
Each custom State class that implements IStateBase must implement the State Property and the 3 declared methods.  This will allow StateManager to call these methods for each State instance when it becomes the current  activeState.

###Scene Logic and Event
Each State class definition contains code to control the scene's logic and event management.  Below is the starter code for the BeginState class, this contains code that finds the EndScene Button GameObject, then creates an objectReference to the Button component.  Then we define a function that we add as a Listener to the Button Component's onClick event.  Finally, we must write the logic we want executed in our custom function which will cause the scene to change and which also instantiates the new State and make this connection with the StateManager because we're calling the StateManager SwitchState method at the same time that we're calling the constructor for the new State.

```
using UnityEngine;
using UnityEngine.UI;
using System.Collections;

public class BeginState : IStateBase {

	// Use this for initialization
	StateManager managerRef;

	private GameState state;

	//add commenets
	public GameState State{
		get{ return state; }
	}

	//GameScene objectRefs
	private Button endBtn;

	//constructor  // add comments
	public BeginState( StateManager manager  ){
		managerRef = manager;  //// establish connection with StateManager
		state = GameState.Begin;

	}

	// Update is called once per frame
	public void StateUpdate () {
        //add code as needed
	}

	public void StateGUI(){
        ///add GUI code as needed
	}
	public void InitializeObjectRefs (){
		endBtn = GameObject.Find ("EndButton").GetComponent<Button> ();
		endBtn.onClick.AddListener (LoadEndScene);///
		Debug.Log ("Add Debug Info");
	}

	public void LoadEndScene(){  
		Debug.Log ("Add Debug Info");
		Application.LoadLevel ("EndScene");
		managerRef.SwitchState (new EndState(managerRef));

	}
}

```


