#Button Example

For this basic example, we'll create some Button and Text gameObjects, and we'll have the Button objects change the text.

We'll create one script:  Controller.cs

**To create a script file, **
   - right-click in the Unity Project panel (bottom panel)
   - select 'Create C# Script' from the pop-up options
   - when the script icon shows up in the Asset panel, name it 'Controller'
   
**To Edit in Visual Studio**
   - Double-click the script icon in the Asset Panel
   - Visual Studio should open with the script displayed
   
**Check** The Filename and the ClassName to make sure they match - otherwise you will get an error when you try to attach the script to a gameObject.

**Code: Declare Library Directives **- To include code libraries - If using UI elements in the script, we must add the following at the top of the file.

   `using UnityEngine.UI;`


**Declare Variables **
   - At the top of the file, we will declare variables that will be connected with gameObjects, components, etc.  The dataType of the variable must match the dataType that the variable will refer to.
   
** In Start( ), make connection between variable and the gameObject's Text component **
   
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

   
   
   
   