#Score / Health / Level Display

In the MiniGame, We want to display the player's score, health and current level.  We'll use UI elements to do this, and we can use the same type of logic we used in the ApplePicker game:

1.  Always Display the Label: Score, Health, Level
2.  Change the value associated with each of these, when a corresponding event happens to change the value.
3.  Complication is that we now have a chain of events, and messaging between GameObjects is required to let the UI-Display elements know when their value needs to be changed.


###Minimize Coupling - Dependencies between GameObjects
These UI elements should be updated when the associated values have changed.  

Score, Health change when the player collides with objects.  This data is stored in GameData, so these objects should register as listeners to the GameData event: OnPlayerDataUpdate.  This requires writing a method that can work as a listener to a UnityEvent.  These methods must have the signature:  


```java
void DoSomething(  no input parameters )
```


