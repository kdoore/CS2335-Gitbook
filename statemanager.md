# StateManager - Singleton Design Pattern

###Learning Goals:
- Learn how to use Singleton Programming Pattern to create persistent GameObjects and Script Objects.
- Learn how to define and use Interfaces like: *IStateBase* to create object references like: `StateManager: activeState` for objects that from classes that implement the interface.
- Learn how to define object references and to pass object references as class Constructor parameters to allow class objects to interact via public methods and public properties. 
- Learn how to instantiate objects by calling the class constructor method.


In Project 2, we have created the StateManager class to manage which state our application is in.  The StateManager inherits from MonoBehavior, so this class can be attached to an empty gameObject as a script component: *GameManager*, and Unity will execute the Start(), Update(), and other Unity event hook functions for the StateManager object instance, which, in-turn will execute StateUpdate() for the currently *activeState*.  

### Singleton Design Pattern
The [Singleton Design Pattern](https://en.wikipedia.org/wiki/Singleton_pattern) is a software engineering paradigm where a class is designed with the understanding that only 1 instance of the class will ever be created. 

We want to insure that an object of this class is only created once, otherwise every time an instance object is created, initialization of the class object would return us to whatever we set as the initial state.  Instead, we want to use one instance of a StateManager object throughout the lifetime of our application. In order to insure that 1 single instance is created when our game is started, and that no other instances are ever created, we can create a special static class reference variable:``StateManager instanceRef; ``, then any other object instances of the StateManager script component that are created are immediately destroyed.


```
public class StateManager : MonoBehaviour {

    public static StateManager instanceRef;  //this will be a reference to the only class object instance allowed to exist
    
	//Called before any GameObjects Start() method is called
	//Allows pre-initialization: Called once when a script is loaded
	void Awake(){
		if(instanceRef==null){  //test to see if instanceRef has been assigned to an object yet.
			instanceRef=this;  //if not, assign it to this current object instance
			Debug.Log ("create me");
			DontDestroyOnLoad(gameObject);  //make sure the gameObject that this is attached to is not destroyed when jumping to a new scene
		}
		else{  //an instance of this class had previously been created.
			Debug.Log ("destroy me");
			DestroyImmediate(gameObject);  //destroy gameObject and the script object instance, to prevent duplication
		}
	}
	
	//additional StateManager code
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
