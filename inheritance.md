# Inheritance
 [MSDN C# Inheritance Reference](https://msdn.microsoft.com/en-us/library/ms173149.aspx)

"Inheritance enables you to create new classes that reuse, extend, and modify the behavior that is defined in other classes. The class whose members are inherited is called the base class, and the class that inherits those members is called the derived class. A derived class can have only one direct base class. However, inheritance is transitive. If ClassC is derived from ClassB, and ClassB is derived from ClassA, ClassC inherits the members declared in ClassB and ClassA." 

In C#, a derived (child) class can inherit from a base (parent) class.  The C# syntax uses a colon :  in the class definition to indicate a class, ``class MyChildClass: Parent`` where ``MyChildClass`` inherits from a ``ParentClass``.  In C#, the same colon syntax, ``:`` is used to indicate that a class is implementing an Interface.  The convention is that all interface names should begin with the letter `I`.  A child class can only inherit from a single base class.  A C# class can implement any number of interfaces and these are a comma separated list, interface example syntax: ``class MyClass: IEnumerable, IEnumerator``. 

``class MyChildClass: ParentClass {  // class definition code  } ``  


## Unity: MonoBehaviour
In order for a C# script to be attached to a game object as a component, the C# class must inherit from the Unity base class: MonoBehaviour.

``` class myScriptClass: MonoBehaviour{  //class definition code
}
```

Since a C# class can only inherit from one base class, any Custom Script which functions as a Unity Component, to add behaviours to a GameObject, can not inherit from any other C# class.

Inheritance