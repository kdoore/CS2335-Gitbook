# State Machine Framework

The diagram below gives an overview of how we will implement a State Machine Framework for a Unity project.

###State Machine - A Design Pattern
State Machines are a fundamental pattern for representing an event-driven system.  We are surrounded by event-driven systems. Understanding how the elegant simplicity of using a State Machine to design event-driven systems is critical for anyone designing interactive media.
[Game Programming Patterns, Robert Nystrom - State Pattern](http://gameprogrammingpatterns.com/state.html) 

[Game Programming Patterns, - Overview: FSM](http://gameprogrammingpatterns.com/state.html#finite-state-machines-to-the-rescue)



 ###State Machine Basics in Unity
 The following link to the Unity Manual gives an overview of how State Machines are used in Unity.
[ State Machines in Unity ](https://docs.unity3d.com/Manual/StateMachineBasics.html)
	
	
	>Unity’s Animation State Machines provide a way to overview all of the animation clips related to a particular character and allow various events in the game (for example user input) to trigger different animations.

###StateManager Class
The StateManager class will manage the stateMachine. It will keep track of the current activeState, it will delegate responsibility for scene logic to the current activeState. It will manage and coordinate messaging and state-transition event synchronization.

###Persistent Object: InstanceRef
The StateManager class will use the Singleton Design Pattern which will insure that only 1 instance: `instanceRef` of this class will exist in our program.  The StateManager class will inherit from MonoBehaviour, this means we can attach this script to an empty GameObject in the starting scene for our program. We'll use the Unity function: `DontDestroyOnLoad(gameObject)` to insure this script component is not destroyed when transitioning between scenes.  

###Keep Track of the ActiveState
We will use the StateManager object to keep track of the current `activeState`.  Finite State Machines require that we maintain a persistent reference to the currently activeState, so this is one responsibility of our StateManager class.

###Pass Control to the ActiveState
We will use the StateManager instance to delegate control of the Scene's logic to the current activeState object instance.  To delegate control, we need to provide the current activeState instance with the ability to have code executed when the Unity Event Functions execute, this will insure the logic code gets executed when necessary.  For each Unity event function that is useful for the activeState to execute, we'll need to provide the activeState a similar event hook, and this will be executed in the StateManager Unity Event Function as discussed using the Unity Update () Event.

##State Machine Framework - Overview Diagram

![](/assets/Cs2335 - 8.png)

###UML Class Diagram - State Machine Framework
![](/assets/Screen Shot 2018-02-12 at 3.43.41 PM.png)


###StateManager.cs Code

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
	private IStateBase activeState;
	private GameScene curScene;

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
	//Replaces depreciated version - OnLevelWasLoaded
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
}
 
```

###State Machine Framework UML Sequence Diagram
The sequence diagram below shows the asynchronous nature of the relationship between Unity Scenes and our custom states,  The StateManager provides a conduit for synchronization communication of event messages between Unity Scenes and our custom states.  
The StateManager receives notification from the Unity engine when a new scene has been loaded, it uses this event ``OnLevelWasLoaded(int levelNumber)`` to send the activeState message, by exectuing the ``activeState.InitializeObjectRefs( )``, so the current activeState can now initialize all object references to gameObjects in the corresponding scene.

![](stateMachineFramework.png)

