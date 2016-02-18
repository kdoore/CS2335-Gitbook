# State Machine Framework

The diagram below gives an overview of how we will implement a State Machine Framework for a Unity project. 

###StateManager Class
The StateManager class will manage the stateMachine. It will keep track of the current activeState, it will delegate responsibility for scene logic to the current activeState. 

###Persistent Object: InstanceRef
We'll use the Singleton Design Pattern which will insure that only 1 instance: `instanceRef` of this class will exist in our program.  The StateManager class will inherit from MonoBehaviour, this means we can attach this script to an empty GameObject in the starting scene for our program. We'll use the Unity function: `DontDestroyOnLoad(gameObject)` to insure this script component is not destroyed when transitioning between scenes.  

###Keep Track of the ActiveState
We will use the StateManager object to keep track of the current `activeState`.  Finite State Machines require that we maintain a persistent reference to the currently activeState, so this is one responsibility of our StateManager class.

###Delegate Control to the ActiveState
We will use the StateManager instance to delegate control of the Scene's logic to the current activeState object instance.  To delegate control, we need to provide the current activeState instance with the ability to have code executed when the Unity Event Functions execute, this will insure the logic code gets executed when necessary.  For each Unity event function that is useful for the activeState to execute, we'll need to provide the activeState a similar event hook, and this will be executed in the StateManager Unity Event Function as discussed using the Unity Update () Event.

###Unity Update( ) Event
Since StateManager inherits from MonoBehaviour, we can implement the Update() function.  In that function we want to call a function for the activeState which will provide the same functionality:

````
///inside StateManager.cs

void Update( ){
    activeState.StateUpdate( );  
}

////inside BeginState.cs

void StateUpdate(){
    doSomethingEveryFrame();
}

```

###Unity Start( ) Event


![](StateMachine.png)



