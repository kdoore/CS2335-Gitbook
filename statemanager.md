# StateManager

In Project 2, we have created the StateManager class to manage which state our application is in.  The StateManager inherits from MonoBehavior, so this class can be attached to an empty gameObject as a script component: *GameManager*, and Unity will execute the Start(), Update(), and other Unity event hook functions for the StateManager object instance, which, in-turn will execute StateUpdate() for the currently *activeState*.  

### Singleton Design Pattern
We want to insure that an object of this class is only created once, otherwise every time an instance object is created, initialization of the class object would return us to whatever we set as the initial state.  Instead, we want to use one instance of a StateManager object throughout the lifetime of our application. In order to insure that 1 single instance is created when our game is started, and that no other instances are ever created, we can create a special static class reference variable:``StateManager instanceRef; ``, then any other object instances that created are immediately destroyed.

###Object Persistence 
Unity proivdes a special method that we can use to insure the GameManager and attached script component: StateManager are not destroyed when execution jumps to a new scene: ``DontDestroyOnLoad(gameObject)``.  Since this StateManager will not be destroyed, then any objects that have an active object reference within the StateManager object will also not be destroyed.  We can use this to keep a reference to an Inventory object, so then the inventory will also have persistence throughout the life of the application,  We may also want to keep track of which states / scenes that we've visited, by maintaining a list of the object references for states which have 

