#State-Controlled UI GameObjects

For the Number game, we want to create a graphical version of the game where the prompts are displayed on the game screen.  We will work in 2D mode, and we need to use a UI game object to display  the text.  This will involve a few steps in order to allow our game script file to modify the UI-Text elements during the game. 

    -  Add a UI -> Text gameObject to the scene
    -  Set the color, position, fontSize of the Text in the inspector, make sure to scale the text area so the text is visible on the screen
    -  Set the initial value of the text in the inspector

###Canvas
When we add a UI Text element to our scene, it also creates a Canvas and an Event Object in the scene.  We won't use the Event object in this project, but we will use the canvas object.  The canvas object is the container for any UI-Text objects in our scene, and our Text object's transform object is defined relative to the canvas since the Text is a child of canvas in the hierarchy panel. 

For this project, let's attach the numberGame.cs script file to the canvas object since we'll be controlling canvas elements from the script, this isn't necessary but it is logical that we'll want to attach scripts to objects we're controlling.

###Code for UI-Text Integration

In our numberGame.cs script, we need to add a new library so that we can access the UI library functions and we also need to create a Text variable that will be linked to the UI Text in the game scene so that it can modify the properties of the UI-Text gameObject.

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
     




