# Interface

C# Interfaces are a concept similar to a C# Class, in that provide a way to implement a set of behaviors across several different classes.  With Classes, child classes inherit all methods that are implemented in the base-class. With an Interface, we are only provide the signature for a method in the interface, we don't provide the actual body / coded functionality of the method.  

 An interface defines a contract: any class that implements an interface, MUST provide implementation for all methods and properties specified in the interface, with in the class definition.  So, in the class that implements the interface, we'll find the actual code for the method's functionality.  When writing an interface, it's assumed that several different classes will implement the interface, with the goal that we can insure that a behavior is implemented across all classes that implement the interface.
 
 An example is that we could have several different types of objects that we'd allow the player to collect.  If we defined an ICollectable interface, and then specified a method: Collect( ). Any class that included (implemented) the interface, would be guaranteed to have Collect( ) implemented, but each could have a different behavior, based on the code included in the body of the method as it's implemented in each class that implements the interface.


[MSDN Programming Guide](https://docs.microsoft.com/en-us/dotnet/csharp/programming-guide/interfaces/)

    >An interface contains definitions for a group of related functionalities that a class... can implement.

    >By using interfaces, you can, for example, include behavior from multiple sources in a class. That capability is important in C# because the language doesn't support multiple inheritance of classes. 

    >You define an interface by using the interface keyword, as the following example shows.

```java
   
interface IEquatable<T>
{
    bool Equals(T obj);
}
```

[Unity video on Interfaces](https://unity3d.com/learn/tutorials/topics/scripting/interfaces)

###Interfaces in Unity


[Unity video on Interfaces](https://unity3d.com/learn/tutorials/topics/scripting/interfaces)

Interfaces provide a way implement similar behaviors across several classes.  Since most scripts we write will have MonoDevelop as the 


