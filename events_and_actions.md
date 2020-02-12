# Events and Actions

We have seen that State-machines provide a simple framework to model complex-dynamic systems such as games and simulations because many dynamic systems can be represented using a set of states, events, and transitions.  We can also consider Actions as a component of State-Machines, where an event may trigger a set of actions to be executed, where any number of these actions can also be considered to trigger an event.  

###Events
When viewing a dynamic system through a State-machine lens: "an event is something that happens which affects the system." [Wikipedia](https://en.wikipedia.org/wiki/UML_state_machine#Events)

###Actions
A state machine responds to events by performing actions.

For example: changing a variable, performing I/O, invoking a function, generating another event instance, or changing to another state. Any parameter values associated with the current event are available to all actions directly caused by that event. [Wikipedia](https://en.wikipedia.org/wiki/UML_state_machine#Actions_and_transitions)

###Event-Driven Programming
Steven Ferg has written an excellent paper, [Event-Driven Programming Introduction, Tutorial, History](http://eventdrivenpgm.sourceforge.net/), which discusses the complexities of programming to create event-driven systems. 

###Event Notification 
Some gameObjects know that they need to be notified when another gameObject does something.  If both objects are in the same scene throughout the scene execution lifetime, then we can create an object reference to the gameObject that wants to be notified of an event, and we can create a method in that gameObject's custom script, that we can call from the object where the event occurs:

GameObject someSubscribeObject = GameObject.Find("SomeObject").GetComponent<CustomScript>();

someSubscribeObject.NotifyMe();

###Event Handlers
Delegates define the syntax for Event handlers.

