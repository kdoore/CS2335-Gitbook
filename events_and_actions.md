# Events and Actions

We have seen that State-machines provide a simple framework to model complex-dynamic systems such as games and simulations because many dynamic systems can be represented using a set of states, events, and transitions.  We can also consider Actions as a component of State-Machines, where an event may trigger a set of actions to be executed, where any number of these actions can also be considered to trigger an event.  

###Events
When viewing a dynamic system through a State-machine lens: "an event is something that happens which affects the system." [Wikipedia](https://en.wikipedia.org/wiki/UML_state_machine#Events)

###Actions
A state machine responds to events by performing actions.

For example: changing a variable, performing I/O, invoking a function, generating another event instance, or changing to another state. Any parameter values associated with the current event are available to all actions directly caused by that event. [Wikipedia](https://en.wikipedia.org/wiki/UML_state_machine#Actions_and_transitions)

###Event Notification

###Event Handlers

