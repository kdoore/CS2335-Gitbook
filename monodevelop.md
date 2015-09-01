#MonoDevelop

MonoDevelop an Integrated Development Environment (IDE) that is installed as part of the Unity 5 installation.  It is an open-source IDE that is integrated to work with the Unity Editor.

To edit a script in MonoDevelop, first create a script component object inside Unity.  Make sure to name the file correctly when creating it in Unity because the script name is used to create the class name of the C# class that is created for each Unity script component.  

##MonoBehavior
MonoBehavior is a C# class that is part of the Unity Engine.  Almost all of the C# classes that we create will inherit from the MonoBehavior class.  This inheritance relationship means that our script component objects can be attached to Unity GameObjects and their Start() and Update() methods will be executed when our GameObject is part of the currently active Unity Scene.  

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
    
##Create Script in Unity
In the animation below, we create a C# script, name it "Controller", and then we double-click the script. This opens the script in MonoDevelop, where we can see that code for the Start() and Update() functions is already included in the file.  We see that we've created a ``class Controller`` that inherits from the ``MonoBehavior`` class.  When we create a public variable, then that variable shows up in the inspector panel in the Unity Editor, so we can easily modify that value.
    
![](createScript.gif)
    
##Attach Script to Camera Object
In the animation below, we select the main camera in the Hierarchy panel, then in the inspector panel we select 'Add Component', then we select 'script'. This lets us select the script that we've already created.  When we push play, the script is executed and we see the ``Debug.Log()`` output in the console panel.  The Console provides a good way to get information about our program during it's execution.
![](attachScript.gif)