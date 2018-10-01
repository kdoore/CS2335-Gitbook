#Singleton Pattern

Singleton is a Programming Design Pattern: 

    > In software engineering, the singleton pattern is a software design pattern that restricts the instantiation of a class to one object. This is useful when exactly one object is needed to coordinate actions across the system
[wikipedia](https://en.wikipedia.org/wiki/Singleton_pattern)

An implementation of the singleton pattern must:

    -    ensure that only one instance of the singleton class ever exists
    -    provide global access to that instance
    
    [wikipedia](https://en.wikipedia.org/wiki/Singleton_pattern)

##Singleton in Unity:
In Unity, Singletons are often used for management of aspects a game such as: state-manager, data-manager, audio-manager, etc.

In Unity, the Singleton Pattern should ensure:
   - only one instance of the object exists 
   - in each scene, it's necessary to destroy any instances that are created as duplicates
   - A global, static variable should be initialized that creates an object-reference to the singleton object.
   - Code to create a Singleton should be located in Awake( );

###StateManager Singleton  
    
 ```java
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

  