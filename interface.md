# Interface

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
