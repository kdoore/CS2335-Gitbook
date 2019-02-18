# IStateBase
IStateBase is a custom interface that we will have all StateX classes implement.  This guarantees that all States will implement all Properties and Methods defined in this interface. 

###C# Interfaces in Unity

Interfaces provide a method to define a set of behaviors that we want to insure are implemented across several classes.  We can only specify that any custom class can only  have 1 parent class, in most of our Unity custom code, MonoBehaviour is that base-class.  So, in order to implement related behavior across different classes, we'll use interfaces, since any class can implement any number of interfaces.   

StateManager,  will use an IStateBase object reference to keep track of the current active state instance.  StateManager will execute the methods  defined the interface IStateBase for the currently activeState.  StateManager delegates responsibility to the current activeState using these methods.

##IStateBase - Why do we need it?

 Here is how we'll use IStateBase, we'll  use it to refer to the currently active state, and this reference will be created and used in the StateManager instance as a way to keep track of the current state.  With this reference to the activeState the StateManager can activate methods (execute functions) on the activeState, even though the states will be changing throughout the course of the game, across different scenes.  This is how we remember where the player is during the game's execution.  We could create a list to keep track of all of the states that the player has visited - we'd want to push some token which represents a state onto an ordered list - then these can be accessed as necessary from other objects.
 
 ###Here's how we use it!

```java
// in StateManager.cs

IStateBase activeState;  //

void Start(){
    activeState = new BeginState( );  //reference to first activeState
    curState = activeState.Scene;
}

```

#Code: IStateBase 

```java
using UnityEngine;
using System.Collections;

///
///  
/// <summary>
/// I state base.  
/// Interface for all StateX.cs classes  
/// </summary>
public interface IStateBase
{

	/// <summary>
	/// Gets the scene number - enum
	/// </summary>
	/// <value>The scene.</value>
	GameScene Scene { // Inteface Property
		get;
	}

	//all interface methods are public by default!

	/// Similar to Unity Start()  
	/// exectued once, after scene is loaded - called from StateManager
	/// Used to initialize object references - can be used to cache object references
	/// </summary>
	void InitializeObjectRefs ();
}

```

###StateX.cs
Each custom State class that implements IStateBase must implement the State Property and the 3 declared methods.  This will allow StateManager to call these methods for each State instance when it becomes the current  activeState.

###Scene Logic and Event
Each State class definition contains code to control the scene's logic and event management.  Below is the starter code for the BeginState class, this contains code that finds the EndScene Button GameObject, then creates an objectReference to the Button component.  Then we define a function that we add as a Listener to the Button Component's onClick event.  Finally, we must write the logic we want executed in our custom function which will cause the scene to change and which also instantiates the new State and make this connection with the StateManager because we're calling the StateManager SwitchState method at the same time that we're calling the constructor for the new State.

#Code BeginState.cs
```java

using UnityEngine.UI;
using System.Collections;
using UnityEngine.SceneManagement;

public class BeginState : IStateBase
{
	/// <summary>
	/// The scene.
	/// </summary>
	private GameScene scene;

	/// <summary>
	/// Gets the scene number - enum
	/// </summary>
	/// <value>The scene.</value>
	public GameScene Scene {
		get{ return scene; }
	}

	//GameScene objectRefs
	private Button endBtn;

	/// <summary>
	/// Initializes a new instance of the <see cref="BeginState"/> class.
	/// </summary>
	/// <param name="manager">Manager.</param>
	public BeginState ()
	{
		scene = GameScene.Begin;
	}

	/// <summary>
	/// Similar to Unity Start() 
	/// exectued once, after scene is loaded - called from StateManager
	/// Used to initialize object references - can be used to cache object references
	/// </summary>
	public void InitializeObjectRefs ()
	{
		endBtn = GameObject.Find ("EndButton").GetComponent<Button> ();
		endBtn.onClick.AddListener (LoadEndScene);
		Debug.Log ("Add Debug Info");
	}

	/// <summary>
	/// Event handler - called when endBtn is clicked
	/// Loads the end scene.
	/// </summary>
	public void LoadEndScene ()
	{  
		Debug.Log ("Add Debug Info");
		SceneManager.LoadScene ("EndScene");  //actual scene name
		StateManager.instanceRef.SwitchState (new EndState ());  //create new state, pass to StateManager
	}
}


```


