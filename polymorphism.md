# Polymorphism

Polymorphism is one of the three major fundamental concepts in Object-Oriented Programming.  The other 2 major OOP concepts are Inheritance and Encapsulation.  Polymorphism is closely-related to inheritance.  

Polymophism has 2 main conceptual components which are detailed in the MSDN reference sections below.  Polymorphism is a run-time concept that controls whether a child-class method is executed, rather than the base-class method of the same name.  In C#, it is necessary to declare a the base-class method as *virtual* if we intend to allow it to be *overridden* by the child class method, when a base-class reference, refers to a child-class object instance.  If no override method is specified in a child class, then the base-class method is executed.

###[Polymorphism: MSDN Reference](https://msdn.microsoft.com/en-us/library/ms173152.aspx)

---

"At run time, objects of a derived class may be treated as objects of a base class in places such as method parameters and collections or arrays. When this occurs, the object's declared type is no longer identical to its run-time type.
Base classes may define and implement virtual methods, and derived classes can override them, which means they provide their own definition and implementation. At run-time, when client code calls the method, the CLR looks up the run-time type of the object, and invokes that override of the virtual method. Thus in your source code you can call a method on a base class, and cause a derived class's version of the method to be executed."

---

###Virtual Methods - Base Class
When a base class method is marked as *virtual*, then this method can be overridden in the child class to allow the child class to provide custom implementation that is more specialized for a child class.  Then, if the method is marked as *override* in the child class, then the child class method is executed at run-time if the object instance is of the child class type.  If the method isn't overridden in the child class, then the base class method is executed.  [Example Code: NPC, Zombie](https://kdoore.gitbooks.io/cs-2335/content/non-player_character_base-class.html) 

```C#
///NPC Base-Class
public virtual void doSomething(){
  Debug.Log("NPC Base Class DoSomething");
}
```


###Override Methods - Child Class
When a child class wants a method to have a specialized implementation, then the child version of a method will be executed if the child method is declared as *override*.

```C#
///Zombie Child-class (extends NPC)
public override void doSomething(){
  Debug.Log("Zombie Child Class DoSomething");
}

///Other Class:
NPC myNPC = new Zombie();
myNPC.doSomething();   // console output:  "Zombie Child Class DoSomething" 
```