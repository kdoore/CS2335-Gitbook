# Inheritance

[MSDN C# Inheritance Reference](https://msdn.microsoft.com/en-us/library/ms173149.aspx)

In C#, a child class can inherit from a parent / base class.  The syntax uses a colon :  in the class definition to indicate a class, ``MyChildClass`` is inheriting from a ``ParentClass``.  In C#, the same syntax is used to indicate that a class is implementing an Interface.  The convention is that all interface names should begin with the letter `I`.  A child class can only inherit from a single base class.  A C# class can implement any number of interfaces. 

``class MyChildClass: ParentClass {  // class definition code  } ``  


## Unity: MonoBehaviour
In order for a C# script to be attached to a game object as a component, the C# class must inherit from the Unity base class: MonoBehaviour.

``` class myScriptClass: MonoBehaviour{  //class definition code
}
```

Since a C# class can only inherit from one base class, any Custom Script which functions as a Unity Component, to add behaviours to a GameObject, can not inherit from any other C# class.

Inheritance