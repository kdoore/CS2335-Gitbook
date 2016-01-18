# MonoBehaviour


MonoBehaviour is a C# class that is part of the Unity Engine.  Almost all of the C# classes that we create will inherit from the MonoBehavior class.  This inheritance relationship means that our script component objects can be attached to Unity GameObjects and their Start() and Update() methods will be executed when our GameObject is part of the currently active Unity Scene. 

## C\# Inheritance
The Syntax for indicating that a class inherits from a base class in C# is shown below, the colon : is used to indicate   
```
public class myClass : BaseClass{
    
}
```

##Script Component Object
When we want to create a C# script to add or control behaviors of a game object, there are many different ways that we can create a C# script. There are also several ways to have our script attached to a Unity GameObject so it can be instantiated and executed as part of our game's code execution.


<quiz>
 <question>
        <p>What is MonoBehavior?</p>
        <answer correct>A base-class for custom-script component classes to add custom behaviors to Unity game objects</answer>
        <answer>A C# interface for that provides Start() and Update() hooks for Unity game objects  </answer>
        <answer>A type of illness that causes fatigue which results from too much time spent programming C# scripts for Unity game objects </answer>
    </question>
    </quiz>
    