Unity Editor
============



Most of the code that we write for this course will end up being attached to gameObjects within the Unity Editor.  

###Sample Code:  
The code section below is a very basic C# script, *Example.cs*.  This creates a C# ``class Example``, which inherits from the ``MonoBehavior`` class.  When creating a C# script object, the code below shows the code that is automatically included in the file when it's created, this includes the MonoBehavior Start() and 
Update() functions. 


```
using UnityEngine;
using System.Collections;
public class Example: MonoBehaviour {
       //Use this for initialization
       void Start () {  //this code is executed one time
            Debug.Log("Initialization has occured");
       }
       //Update is called once per frame
       void Update () {  //this code is executed once each frame
            Debug.Log("Update Called");
        } 
}```