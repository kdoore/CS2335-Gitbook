# StateManager

In Project 2, we have created the StateManager class to manage which state our application is in.  The StateManager inherits from MonoBehavior, so this class can be attached to an empty gameObject: GameManager, and Unity will execute the Start(), Update(), and other Unity event hook functions.  

### Singleton Design Pattern
We want to insure that this class is only created once, otherwise everytime it is created, the constructor initialization would return us to whatever we set as the initial state.  Instead, we want to use one instance of the StateManager class throughout the lifetime of our application. In order to insure that 1 single instance is created when our game is started, and that no other instances are ever created, we can create a special static class reference variable:``StateManager instanceRef; ``, and 

###Object Persistence 
Unity proivdes a special method that we can use to insure the GameManager and attached script component: StateManager are not destroyed when execution jumps to a new scene: ``DontDestroyOnLoad(gameObject)``

