#Number Game: UI GameObjects

For the Number game, we want to create a graphical version of the game where the prompts are displayed on the game screen.  We will work in 2D mode, and we need to use a UI game object to display  the text.  This will involve a few steps in order to allow our game script file to modify the UI-Text elements during the game. We'll modify NumberGame.cs to integrate the UI GameObjects, the section below lists the steps we need to complete.  [See Screenshot Animation 1 Below](https://kdoore.gitbooks.io/cs-2335/content/state_controlled_ui-text.html#animations)

-  GameObject -> Add a UI -> Text gameObject to the scene

    -  This creates a Canvas gameObject in the Hierarchy Panel
    -  This creates an EventSystem in the Hierarchy Panel
    -  Double-click the text element to find it within the scene.
    - Give it a unique name in the inspector
    -  Set the color, position, fontSize of the Text in the inspector, make sure to scale the text area so the text is visible on the screen
    -  Set the initial text value of the text component in the inspector
    -  In NumberGame.cs, add a library reference: using UnityEngine.UI at the top of your code
    -  Write code in the NumberGame.cs file to declare a public Text variable, initialize and modify in the script file.
    -  Connect script-component public text-variable and UI-text elements in the inspector on the gameObject that has the numberGame.cs script component attached.
    
###Unity Tutorials on the UI Components
[Unity UI Tutorials Link](https://unity3d.com/learn/tutorials/topics/user-interface-ui)

###Canvas and Event System 
When we add a UI Text element to our scene, it also creates a *Canvas GameObject* and an *EventSystem GameObject* in the scene.  We won't use the EventSystem object in this phase of the project, but it's important to realize this is required for any user-interaction with UI elements, when copying UI elements between scenes, always make sure the new scene has an EventSystem GameObject in the Hierarchy - this is a difficult error to debug.  The Canvas object is the container for any UI-Text objects in our scene, and our Text object's transform object is defined relative to the canvas since the Text is a child of Canvas in the Hierarchy panel. Adding additional UI elements to the scene does cause additional Canvas or EventSystem gameObjects to be added to the scene. 

For this project, let's attach the NumberGame.cs script file to the canvas object (or attach to the main-camera Game Object) since we'll be controlling several Text-UI objects which are all contained in the canvas game object from the script, this isn't necessary but it is logical that we'll want to attach scripts to objects we're controlling.

###Code for UI-Text Integration

In our NumberGame.cs script, we need to add a new UnityEngine code library so that we can access the UI library functions. We also need to create a script Text variable for each UI-Text GameObject that will be linked to the UI Text in the game scene so that it can modify the properties of the UI-Text GameObject. We need to make sure our Text variables are declared as public instance variables so that they will show up in the script-component section of the inspector panel for the object that we've attached our script to.  

```Csharp
using UnityEngine;
using UnityEngine.UI;  //make sure to add Unity UI library
using System.Collections;


public class NumberGame : MonoBehaviour {
	public Text gameText;  //game dialog
	public Text numberText;  //guessed number
	public Text instructionText;  //input instructions
	
	}
	
```

Next, go back into Unity, select the Canvas object in the Hierarchy Panel, (make sure we've attached our script to the Canvas GameObject), then we need to connect our TextUI elements with the public instance variables from our script component. We can do this by either dragging the UI-Text object from the hierarchy panel to the script component text-box, or we can select the text element in the script component, select the dot to the right of the text field and it will open a pop-up of all possible game-objects that we can select to connect to this script variable element.   [See Screenshot Animation 2 Below](https://kdoore.gitbooks.io/cs-2335/content/state_controlled_ui-text.html#animations)


###Connecting C# Text reference variables to Unity UI-Text components.
In the NumberGame.cs file, we need to write code to modify the UI text elements.  We've declared the public Text elements as class instance variables.  Now in the code we want to modify the Text.

We can initialize the text elements in Start()

```Csharp
void Start(){  //initialize text values
    gameText.text="Want to play a game?" ;
    numberText.text="";  //set text to empty string when not in use
    instructionText="Enter Y to play or N to quit";
````

### Animations - Connect Code and GameObject/Components using the Inspector 
1. How to Create Text-UI Game Objects and Attach Script to Canvas Object
2. How to Connect Text-UI Game Object to Script Text Element


![](GU6iOIPXxo.gif)


In the animation below, we have 2 UI-Text GameObjects: StateText and GameText. We want to link them with 2 class instance-variables, stateText and instructText, that are contained in a C# class file: StateController.cs.  This script component for this C# class is attached to the Canvas GameObject, so we first need to select the Canvas object in the Hierarchy, then we find the variables in script-component in the inspector.  Then we can either drag the UI-Text GameObjects, or we can select the tiny circle tool immediatly to the right of the text variable name which will open a new panel with all GameObjects that match the same type in our scene.

![](jfawLfwFA0.gif)

###Connect to GameObjects and Components using code.

In the animations and descriptions above, we have used the inspector to make a connection between our gameObject components and our code references.  While this might seem like the easiest approach, we are actually better of to do the connection within the Start() function of our code file:  We need to do the following things:


* 
Declare a reference variable that matches the component type that we want to modify in our code.  The variable should be declared as an instance variable for the class: 

```cSharp
///class instance variables
Text promptText;
```

* 
In Start( ), we need to find the gameObject and access the Text component.
 
```CSharp
 promptText = GameObject.Find ("PromptText").GetComponent<Text> ();
		
 ```
 
* 
Then we need to initialize the text field of the Text component, and we'll modify this value throughout the rest of the game code

```C#
promptText.text = "Do you want to play a game";
 ```
 
* 
If we need to include numbers as part of our output, we can use the string.Format( ) function as below.

```C#
string promptString = string.Format ("Think of a number between {0} and {1}", min, max); 

promptText.text = promptString;		
```

Throughout our program, we'll modify the value of promptText.text, so we provide correct output to the player.