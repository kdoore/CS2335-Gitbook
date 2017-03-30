#Delegates

Delegates are useful in Unity because delegates allow creation of custom event handlers. Event-handlers are methods that are executed when some event has occurred.  Since Unity is an Event-driven system, events, event notifications, and event-handlers are used to create dynamic behavior of gameObjects within a game.  Spawning prefabs is a common situation where it is useful to be able to create custom events and event-handlers using custom delegates.  

[MSDN Definition](https://msdn.microsoft.com/en-us/library/edzehd2t(v=vs.110):
"A delegate is a type that holds a reference to a method. A delegate is declared with a signature that shows the return type and parameters for the methods it references, and can hold references only to methods that match its signature. A delegate is thus equivalent to a type-safe function pointer or a callback. A delegate declaration is sufficient to define a delegate class."

Details: 
* A delegate declaration defines a special type of a reference, which means it can hold a memory address of a method.

* A delegate declaration specifies the signature of a function / method
  * return type
  * name
  * input parameter list 


 ``` public delegate void SomeEventHandler (  );   //delegate declaration```
 

*  A delegate instance is declared using the name of the delegate-type     

``` public SomeEventHandler handleEvent; ```



[Delegate Tutorial](http://unitydojo.blogspot.com/2015/03/how-to-use-delegates-in-unity-like-boss.html) by Juan Gomez 





