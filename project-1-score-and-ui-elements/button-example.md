#Simple Script - Text 

For this basic example, we'll create a Text UI gameObject, later we'll add Button objects to change the text.

**Name and Save Scene** 
Always start by giving your scene a meaningful name, then save the scene, this scene name should now be displayed in the hierarchy panel. 

**Create UI-Text GameObject in Unity**
In The Unity Hierarchy Panel, add a UI-Text element.   This should create 3 GameObjects:
   - Canvas
   - Text
   - EventSystem 
   
**Configure Text GameObject**
   - Rename to:  DisplayText
   - Set color so it's visible
   - change font size, or select BestFit, so it's visible
   - Change default text to: "Hello"
   - Set paragraph alignment as desired
   - Set the Rect-Transform so the text is anchored to stay within the canvas.
   - [Link ](/screen-space_canvas.md)To directions for changing the Canvas RenderMode to Screen-Space Camera

**Create C# script:  Controller.cs**

**To create a script file, **
   - right-click in the Unity Project panel (bottom panel)
   - select 'Create C# Script' from the pop-up options
   - when the script icon shows up in the Asset panel, name it 'Controller'
   
   
   
**To Edit in Visual Studio**
   - Double-click the 'Controller' script icon in the Asset Panel
   - Visual Studio should open with the script displayed
   
**Check** The Filename and the ClassName to make sure they match - otherwise you will get an error when you try to attach the script to a gameObject.

**Code: Declare Library Directives **- To include code libraries - If using UI elements in the script, we must add the following at the top of the file.

   `using UnityEngine.UI;`


**Declare Variables **
  At the top of the file, we will declare variables that will be connected with gameObjects, components, etc.  The dataType of the variable must match the dataType that the variable will refer to.
  
  `Text displayText; `
   
   **Initialize Variables in Start()**
 In Start( ), make connection between variable and the gameObject's Text component
 In one line of code, we do 2 things:
  -  Find GameObject 
  -  make connection with the Text component

   
```
displayText = GameObject.Find("DisplayText").GetComponent<Text>(); 
    
```
**Change the Value for the Text component's 'text' field: **

```
displayText.text = "Goodbye";
```

**Debugging**
Return to Unity after saving or building the file changes in Visual Studio. Play the scene by pressing the play button at the top of the Game Panel.
If the text doesn't change:
1.  Did you add the script to the canvas gameObject to create a Script Component?
2.  Null-Reference exception - means there's a mismatch between the GameObject name in Unity and in your script.
3.  If you can't add the script to the Canvas GameObject, then there's probably a mismatch between the script's file-Name and the class-Name in the script.  


   
#Controller.cs Code

```java
using System.Collections;
using System.Collections.Generic;
using UnityEngine;
using UnityEngine.UI;


public class Controller : MonoBehaviour {

    Text displayText;  //declare a variable that will refer to the Text component on the DisplayText gameObject in the scene  

	// Use this for initialization
	void Start () {
	//initialize the variable, First find the GameObject named "DisplayText, then connect to the Text component on that gameObject
        displayText = GameObject.Find("DisplayText").GetComponent<Text>(); 
        //change the value of the text field to be displayed in Unity
        displayText.text = "Goodbye";
	}
	
	// Update is called once per frame
	void Update () {
		
	}
}


```

#Save your Scene
The logic we have just configured is contained in our default Unity Scene.  We need to save the scene by giving it a unique name, we should always save the scene we're working on before leaving the scene or quitting Unity. 



   
   
   
   