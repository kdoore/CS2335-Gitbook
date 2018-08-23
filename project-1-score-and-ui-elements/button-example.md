#Simple Script - Text & Buttons

For this basic example, we'll create a Text UI gameObject, and we'll add Button objects to change the text.

We'll create one script:  Controller.cs

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



   
#Unity Code 

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

   
   
   
   