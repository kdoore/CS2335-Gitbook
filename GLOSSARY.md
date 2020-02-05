# Glossary

### Enumeration

The **enum** keyword is used to declare an enumeration-type, which creates a distinct type that consists of a set of **named constants** called the enumerator list. Since ``enums`` are **constant values**, once they have been defined, they can't be modified within code. The underlying data-type for ``enums`` is ``int``. To use an enum, first define a varialble of the enum-type, then use dot notation to assign a particular enum value to the variable: [Enum: MSDN Reference](https://msdn.microsoft.com/en-us/library/cc138362.aspx)

```java
//define an enum
enum Days { Sunday, Monday, Tuesday, Wednesday, Thursday, Friday, Saturday };  

//use an enum
Days currentDay = Days.Sunday;

//to access the integer value of an enum, use type-casting;
Debug.Log("current Day integer value " + int(currentDay));
```

### Interface

### Inheritance 
In C#, a child class can inherit from a parent / base class.  The syntax uses a colon :  in the class definition to indicate a class, ``MyChildClass`` is inheriting from a ``ParentClass``.  In C#, the same syntax is used to indicate that a class is implementing an Interface.  The convention is that all interface names should begin with the letter `I`.  A child class can only inherit from a single base class.  A C# class can implement any number of interfaces. 

``class MyChildClass: ParentClass {  // class definition code  } ``  

### C# Generic Types  < T >
Generics introduce the concept of type parameters to the .NET Framework, which make it possible to design classes and methods that defer the specification of one or more types until the class or method is declared and instantiated by client code.
[MSDN Reference](https://docs.microsoft.com/en-us/dotnet/csharp/programming-guide/generics/)

Unity Example: **GetComponent < T >(  )**

```java
private Animator animator;
animator = GetComponent<Animator>();  //GetComponent< T >( ) Generics: Placeholder T

```

### C# Static
The keyword `static`, when applied to a property, variable,  method, means that the element belongs to the class itself, and not an object instance of the class.

Example: **Class: Mathf, method: Abs(  float val )**

```java

 float inputX = Input.GetAxis("Horizontal");  // -1, 0 , 1 
 bool isWalking = Mathf.Abs(inputX) > 0;

```




###SerializeField Attribute
SerializeField attribute allows you to have private script variables that will be visible in the Inspector. This allows setting values in the editor without giving access to the variable from within other scripts.
  

```java
 [SerializeField]
    private string description;
```

