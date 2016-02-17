# State Machine Framework

The diagram below gives an overview of how we will implement a State Machine Framework for a Unity project. 

We will create the StateManager class to manage the stateMachine.  We'll use the Singleton Design Pattern which will insure that only 1 instance of this class will exist in our program.  We'll attach this script to an empty GameObject in the starting scene for our program, and we'll use the Unity function: `DontDestroy 

![](StateMachine.png)



