# Observer Pattern
The Observer Pattern is a programming pattern that reflects common game-design situations, as discussed in the book: [Game Programming Patterns.](http://gameprogrammingpatterns.com/observer.html)  The situation is that we have an event that occurs within one component, the Subject, and we have other components, Observers, that want to be notified when this event has occurred.  Often the Observer Pattern is implemented using Interfaces, however, since Unity uses C# and .Net framework, then we can use C# delegates and events to create a simpler Observer Pattern implementation.

###Subject:


###Observer

###MVC Pattern
The observer pattern is similar to the Model-View-Controller pattern, where the model is an abstract representation of the data for the system, the view  represents the interface-view that is rendered for the user.  The view should be automatically notified when there are changes in the data-model, so it can update the user's view of the system.  

###Publish / Subscribe System
We can also view this situation, we need one object to publish notification of events, while other objects are configured as subscribing for notification of events.  

###Loose Coupling
These patterns provide a way to reduce dependencies between objects within a system.  The publisher / subject does not need any details about which objects have subscribed for notification of events.  This means that design changes to the system will be less likely to 



