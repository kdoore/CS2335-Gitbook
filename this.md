#this keyword

The `this` keyword provides a way for the object instance that is currently executing code to refer to itself.  

##this in Unity
In Unity, most custom code that we write is to create script components.  Therefore, in Unity, `this` usually refers to the script component that is executing the current code. Common uses include accessing a different component, or identifying the associated gameObject.  

```java
Vector3 pos = this.transform; //example of accessing a component

Destroy( this.gameObject);  //example of identifying associated gameObject

```


[MSDN Reference](https://docs.microsoft.com/en-us/dotnet/csharp/language-reference/keywords/this)

>The this keyword refers to the current instance of the class ...

The MSDN reference provides several usage examples
