#Event Publishing Design Patterns
There are several similar approaches to designing a system where we can have our custom objects communicate with each other when events occur. Many Unity UI GameObject Components provide this type of event messaging functionality, but we want to understand these programming patterns so we can create this functionality with our own objects.

###UI-Buttons
In Unity, we have worked with UI Buttons, where we create a reference to a UI-Button component in our custom C# class script, then we add our custom class method as a listener to the button component's onClick event, using code similar to that below.
In the code below, we have defined a function: void GameStartActions() that we want to have exectued when the button click() event occurs.  We are able to 

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
The Observer Pattern is an OOP programming pattern that reflects common game-design situations, as discussed in the book: [Game Programming Patterns.](http://gameprogrammingpatterns.com/observer.html)  The situation is that we have an event that occurs within one component, the Subject, and we have other components, Observers, that want to be notified when this event has occurred.  Often the Observer Pattern is implemented using Interfaces, however, since Unity uses C# and .Net framework, then we can use C# delegates and events to create a simpler Observer Pattern implementation.

###Subject:


###Observer

###MVC Pattern
The observer pattern is similar to the Model-View-Controller pattern, where the model is an abstract representation of the data for the system, the view  represents the interface-view that is rendered for the user.  The view should be automatically notified when there are changes in the data-model, so it can update the user's view of the system.  

###Publish / Subscribe System
We can also view this situation, we need one object to publish notification of events, while other objects are configured as subscribing for notification of events.  

###Loose Coupling
These patterns provide a way to reduce dependencies between objects within a system.  The publisher / subject does not need any details about which objects have subscribed for notification of events.  This means that design changes to the system will be less likely to 



