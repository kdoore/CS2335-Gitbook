#Event Publishing Design Patterns
There are several similar approaches to designing a system where we can have our custom objects communicate with each other when events occur. Many Unity UI GameObject Components provide this type of event messaging functionality, but we want to understand these programming patterns so we can create this functionality with our own custom objects.

###Example: UI-Buttons - onClick.AddListener
In Unity, we have worked with UI Buttons, where we create a reference to a UI-Button component in our custom C# class script, then we add our custom class method as a listener to the button component's onClick event.
In the code below, we have defined a function: void GameStartActions() that we want to have executed when the button click() event occurs.  It is required that our function match the delegate-type: a function with  a void return-type and no input parameters, in order to add our GameStartActions as a listener to the button's onClick event.  This is the pattern of behavior that we want to create within our code. When the button object is clicked, we want our custom object to be notified, we do so by creating a function that is passed as a delegate to the button's onClick event.

```
private Button gameStartButton;

void Start(){
gameStartButton = GameObject.Find ("StartGame").GetComponent<Button> ();
gameStartButton.onClick.AddListener (GameStartActions);
	}

void GameStartActions(){
	// do game start things
}
```

###Observer Pattern
The [Observer Pattern](https://en.wikipedia.org/wiki/Observer_pattern) is a behavioral programming pattern that is useful for game-design situations, as discussed in: [Game Programming Patterns.](http://gameprogrammingpatterns.com/observer.html)  In a situation where have an event that occurs within one object component, the Subject, and we have other components, Observers, that want to be notified when this event has occurred.  Often the Observer Pattern is implemented using Interfaces, however, since Unity uses C#, then we can use C# delegates and events to create a our Observer Pattern implementation.

###Subject:
The subject is the object instance that will publish notification that an event has occurred, he will maintain a list of other objects that have requested to be notified of the event.  This can represent a "one - to - many" relationship, where there is one subject that notifies many observer objects.

###Observer
An observer is an object instance that has actions to perform once an event has occurred on the subject object, it has subscribed, or registered, to be notified when the event occurs so it can perform it's actions.  

###MVC Pattern
The observer pattern is similar to the Model-View-Controller pattern, where the model is an abstract representation of the data for the system. The view  represents the interface-view that is rendered and displayed for the user. The controller is a component that manages input and communicates with the data-model based on user-event interaction. The view component should be automatically notified when there are changes in the data-model, so it can update the user's view of the system.  

###Publish / Subscribe System
We can consider these design patterns as simplified versions of a publish / subscribe system.  Typically in a Publish / Subscribe system is implemented using an architecture with some type of message-manager class which functions to manage published events and distributes notifications to the subscribing components.

###Loose Coupling
These patterns provide a way to reduce dependencies between objects within a system.  The publisher / subject does not need implementation details about which objects have subscribed for notification of events.  This means that design changes to the system will be less likely to cause errors. Interfaces provide one means of implementing loose coupling between components because it insures that functionality is implemented across all classes that implement an interface, so no details of a classes implementation are required when executing interface methods. Delegates also provide a method of implementing loose coupling because delegates allow passing functions between objects as a means of communicating. 



