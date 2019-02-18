# StateManager - Singleton Design Pattern

###Learning Goals:
- Learn how to use Singleton Programming Pattern to create persistent GameObjects and Script Objects.
- Learn how to define and use Interfaces like: *IStateBase* to create object references like: `StateManager: activeState` for objects that from classes that implement the interface.
- Learn how to define object references and to pass object references as class Constructor parameters to allow class objects to interact via public methods and public properties. 
- Learn how to instantiate objects by calling the class constructor method.


#StateManager Code

```java
using UnityEngine;
using UnityEngine.UI;
using System.Collections;
using UnityEngine.SceneManagement;

///
/// CS2335 - Spring 17
///  
/// <summary>
/// Game Scene. Matches Unity Scenes in Build Settings
/// </summary>
public enum GameScene
{
	Begin = 0,
	End = 1
}

/// <summary>
/// State manager.  Add comments
/// </summary>
public class StateManager : MonoBehaviour
{
	public static StateManager instanceRef;
	public IStateBase activeState;
	public GameScene curScene;

	/// <summary>
	/// Create this singleton instance.
	/// Implement the Singleton Design Pattern - initialize managerInstance
	/// </summary>
	void Awake ()
	{
		if (instanceRef == null) {
			instanceRef = this;
			DontDestroyOnLoad (gameObject);  //the gameObject this is attached to 
		} else {   //
			DestroyImmediate (gameObject);   
			Debug.Log ("Destroy GameObject");
		}

	}
	// Use this for initialization
	void Start ()
	{
		activeState = new BeginState ();
		curScene = GameScene.Begin;
		activeState.InitializeObjectRefs ();  //call to Initialize BeginState object references

		//next scene change - this event will call OnLevelFinishedLoading custom function
		SceneManager.sceneLoaded += OnLevelFinishedLoading;  //add function to sceneLoaded delegate

	}

	/// <summary>
	/// Switchs the state.
	/// </summary>
	/// <param name="newState">New state.</param>
	public void SwitchState (IStateBase newState)
	{
		activeState = newState;
		curScene = newState.Scene;
		Debug.Log ("Add Debug Info");
	}



	//StateManager needs to know when a new scene has been loaded
	//then it can call: InitializeObjectRefs for the current state
    void OnLevelFinishedLoading (Scene scene, LoadSceneMode mode)
	{
		int sceneID = scene.buildIndex;
		if (sceneID == (int)curScene) {
			Debug.Log ("New function in StateManager to initialize new scene objects - exectued");
			activeState.InitializeObjectRefs ();
		} else {
			Debug.Log ("Big Problems - Scene & State Mismatch");
			Debug.Log ("LevelFinished Loading :Scene Loaded: " + sceneID + " ActiveState Scene Enum: " + activeState.Scene);
		}


	}
} //end StateManager

	
```

###Object Persistence 
Unity proivdes a special method that we can use to insure the GameManager and attached script component: StateManager are not destroyed when execution jumps to a new scene: ``DontDestroyOnLoad(gameObject)``.  Since this StateManager will not be destroyed, then any objects that have an active object reference within the StateManager object will also not be destroyed.  We can use this to keep a reference to an Inventory object, so then the inventory will also have persistence throughout the life of the application,  We may also want to keep track of which states / scenes that we've visited, by maintaining a list of the object references for states which have been instantiated.  We'd need to prevent creation of a new state instance each time we return to a scene.  

###StateManager ObjectReferences
When each ``state`` object is initialized, it's constructor has an input parameter that is an object reference to the singleton instance of the StateManager object, this gets assigned to a local reference variable: `manager`. Later, we pass that address along to the next state object when it is time to change states by calling the constructor for the next state.  The SwitchState process is interesting because the ``SwitchState() method belongs to the StateManager object, but the code is executed from within the current state, but the function requires the current state to call the function by calling the constructor method for the next state.  This code really illustrates the fact that methods are used by objects to communicate, they act to allow messages to be sent between objects. 

Class Design and some code for the StateManager , IStateBase, and StateX classes is based on projects in the book: Learning C# by Developing Games with Unity 3D Beginner's Guide, Terry Norton.  PacktPub eBook Version

###StateManager - SwitchState( )
The SwitchState process is quite interesting because the `SwitchState()` method belongs to the StateManager object, but the code is executed from within the current state, but the function requires the current state to call the function by calling the constructor method for the next state.  This code really illustrates the fact that methods are used by objects to communicate, they act to allow messages to be sent between objects. 
The UnityEngine provides methods that allow easy message communication between objects, some of these methods are FindGameObject(), AttachListener(), OnTriggerEnter2D(), etc. [Unity GameObject.Find Documentation](http://docs.unity3d.com/ScriptReference/GameObject.Find.html)

Code in BeginState to switch state and scene to correspond to the EndState, EndScene.  To do that, first we are assuming that the LoadEndScene( ) method will be activated when the endButton is clicked.  Once that happens, we use the Unity SceneManagement's SceneManager static method: LoadScene( "EndScene" ) , here we pass in the name of the Unity Scene that we want to go to.  Next, we use the singleton instance for the StateManager so we can call StateManager method: SwitchState().  When calling SwitchState, we first call the constructor for the new state: new EndState( ), and this object is passed as an input parameter for SwitchState( ), this is how StateManager is able to update the value of activeState, so it will now point to the newly created state.
These 2 actions must be performed each time we want to leave 1 scene and go to another scene.


```java

public void LoadEndScene ()
{
Debug.Log ("Add Debug Info");
SceneManager.LoadScene ("EndScene"); //actual scene name
StateManager.instanceRef.SwitchState (new EndState ()); //create new state, pass to StateManager
}
```
