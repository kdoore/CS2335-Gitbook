# MonoBehaviour


MonoBehaviour is a C# class that is part of the Unity Engine.  Almost all of the C# classes that we create will inherit from the MonoBehavior class.  This inheritance relationship means that our script component objects can be attached to Unity GameObjects and their Start() and Update() methods will be executed when our GameObject is part of the currently active Unity Scene. 

## C\# Inheritance
The Syntax for indicating that a class inherits from a base class in C# is shown below, the colon : is used to inheritance from a base class, MonoBehaviour is the base-class for all Unity MonoBehaviour Components.

 ```java
using UnityEngine;
using System.Collections;

public class Example: MonoBehaviour {
 
  //Use this for initialization
  void Start () { //this code is executed one time
    Debug.Log("Initialization has occured");
  }
  //Update is called once per frame
  void Update () { //this code is executed once each frame
    Debug.Log("Update Called");
  }
}
```


## C# Interfaces
C# Interfaces are similar to Class Inheritance in C#. C# Interfaces can be implemented to provide similar behavior across different C# classes.  
We'll discuss interfaces later in the course.  For now it's important to note that in C#, a class can only inherit from a single base class, but a class can implement multiple interfaces. Unity's Component-Oriented architecture benefits from using Interfaces for reasons listed in the article quoted below.

>  "Interfaced-based design **provides loose coupling, true component-based programming, easier maintainability and it makes code reuse much more accessible** because implementation is separated from the interface."   [Matthew Cochran](https://www.c-sharpcorner.com/article/C-Sharp-interface-based-development/)

```

public class myClass : BaseClass, ISomeInterface, ISomeOtherInterface{  //single level of inheritance, unlimited interface implementation, a comma separated list.
    
}
```

##Custom Script-Component Objects
When we want to create a C# script to create or modify behaviors of a game object, the most common approach is to have our custom class inherit from MonoBehaviour.  Most of the code that we write for this course will end up being attached to gameObjects within the Unity Editor.

Note: there are other ways to have custom code integrated within Unity, we'll look at these other approaches later in the course.

#Create a Script Component
To create a custom script component, create a folder name it 'Scripts' within your Project Panel's Asset folder.
Next, create a **C# script ** by right-clicking inside within the 'Scripts' folder, select the top menu option: C# Script. Insure that the script's Filename is spelled the same as the Class name, otherwise Unity won't allow the script to be added as a component to a game object.


###Sample Code:
The code section below is a very basic C# script, *Example.cs*. This creates a C# ``class Example``, which inherits from the ``MonoBehavior`` class. When creating a C# script object, some code is automatically generated in the file when it's created, this includes the MonoBehaviour Start() and
Update() functions, the Debug.Log() content has been added to the following script to create custom behavior.

```java
using UnityEngine;
using System.Collections;

public class Example: MonoBehaviour {
 
  //Use this for initialization
  void Start () { //this code is executed one time
    Debug.Log("Initialization has occured");
  }
  //Update is called once per frame
  void Update () { //this code is executed once each frame
    Debug.Log("Update Called");
  }
}
```

##Attach Script to Camera Object
In the animation below, we select the main camera in the Hierarchy panel, then in the inspector panel we select 'Add Component', then we select 'script'. This lets us select the script that we've already created. When we push play, the script is executed and we see the ``Debug.Log()`` output in the console panel. The Console provides a good way to get information about our program during it's execution.


C# Interfaces
    