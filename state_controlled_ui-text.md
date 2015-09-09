#State-Controlled UI GameObjects

For the Number game, we want to create a graphical version of the game where the prompts are displayed on the game screen.  We will work in 2D mode, and we need to use a UI game object to display  the text.  This will involve a few steps in order to allow our game script file to modify the UI-Text elements during the game. 

    -  Add a UI -> Text gameObject to the scene
    -  Set the color, position, fontSize of the Text in the inspector, make sure to scale the text area so the text is visible on the screen
    -  Set the initial value of the text in the inspector

###Canvas
When we add a UI Text element to our scene, it also creates a Canvas and an Event Object in the scene.  We won't use the Event object in this project, but we will use the canvas object.  The canvas object is the container for any UI-Text objects in our scene, and our Text object's transform object is defined relative to the canvas since the Text is a child of canvas in the hierarchy panel. 

For this project, let's attach the numberGame.cs script file to the canvas object since we'll be controlling canvas elements from the script, this isn't necessary but it is logical that we'll want to attach scripts to objects we're controlling.

###Code for UI-Text Integration

In our numberGame.cs script, we need to add a new library so that we can access the UI library functions and we also need to create a Text variable for each UI-Text object that will be linked to the UI Text in the game scene so that it can modify the properties of the UI-Text gameObject. We need to make sure our Text variables are declared as public instance variables so that they will show up in the script-component section of the inspector for the object that we've attached our script to.  

````java
using UnityEngine;
using UnityEngine.UI;  //make sure to add Unity UI library
using System.Collections;


public class NumberGame : MonoBehaviour {
	public Text gameText;  //game dialog
	public Text numberText;  //guessed number
	public Text instructionText;  //input instructions
	
	//the rest of our NumberGame code
	}
````

Now, we can go back into Unity, select the canvas object, make sure we've attached our script to the canvas object, then we need to connect our TextUI elements with the public instance variables from our script component. We can do this by either dragging the UI-Text object from the hierarchy panel to the script component, or we can select the text element in the script component, select the dot to the right of the text field and it will open a pop-up of all possible game-objects that we can select to connect to this script variable element.




