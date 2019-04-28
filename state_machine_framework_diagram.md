# State Machine Framework

![](StateMachine.png)

State Machines provide a framework to manage the logic of an event-driven dynamic system.  Many aspects of game-design/development can be modeled and implemented by state-machines.  For   visual novels, we can consider each Scene as a game-state, where player decisions are the events that allow change between scenes.  State Machines provide a simplified perspective of a complex system, which simplifies the logic required to implement dynamic game logic.  

##State Machine is a System Model
A State Machine models a complex system which is simplified according to these rules:  
 - There exists a finite set of well-defined states that the system can be in.  
 - There exists a finite set of discrete events which can cause the system to transition into a different state
 - There is a well-defined set of State-Event relationships that specify valid transitions for the system.
 - Events can be considered as enternal signals that impact the system.
 - It is necessary to specify the starting state of the system
 - It is necessary to have program memory which maintains track of the current system state.

 We implement the StateManager class to manage the State Machine structure.  
 
 State-Machines structure: 
 -  a finite set of states, 
 -  a well-defined set of events that correspond to transitions between states 
 - tracking the currently active state
 
 State:
A State can represent a wide range of things:  a scene, an animation clip, a behavior of an NPC.  

Event:  
 Events can be due to a variety of causes such as user input or due to gameObjects interacting with each other.  
 
 State-Event Transitions: A logical structure specifies which events cause valid transitions for each CurrentState-Event-NextState transition relationship.

###UML Class Diagrams: State-Manager System
**State-Machine Framework**

![](/assets/Screen Shot 2019-04-28 at 11.01.13 AM.png)


