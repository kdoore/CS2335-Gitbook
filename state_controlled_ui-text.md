#State-Controlled UI GameObjects

For the Number game, we want to create a graphical version of the game where the prompts are displayed on the game screen.  We will work in 2D mode, and we need to use a UI game object to display  the text.  This will involve a few steps in order to allow our game script file to modify the UI-Text elements during the game. We'll create a simple example of UI-Text script called StateController.cs, once we understand how to create UI-Text and control it with code for this simplified example, we can integrate these changes into the NumberGame.cs C# script.  [See Screenshot Animation 1 Below](https://kdoore.gitbooks.io/cs-2335/content/state_controlled_ui-text.html#animations)

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

###Canvas and Event System 
When we add a UI Text element to our scene, it also creates a *Canvas GameObject* and an *EventSystem GameObject* in the scene.  We won't use the EventSystem object in this phase of the project, but it's important to realize this is required for any user-interaction with UI elements, when copying UI elements between scenes, always make sure the new scene has an EventSystem GameObject in the Hierarchy - this is a difficult error to debug.  The Canvas object is the container for any UI-Text objects in our scene, and our Text object's transform object is defined relative to the canvas since the Text is a child of Canvas in the Hierarchy panel. Adding additional UI elements to the scene does cause additional Canvas or EventSystem gameObjects to be added to the scene. 

For this project, let's attach the stateController.cs script file to the canvas object (instead of the text or main camera game objects) since we'll be controlling several Text-UI objects which are all contained in the canvas game object from the script, this isn't necessary but it is logical that we'll want to attach scripts to objects we're controlling.

###Code for UI-Text Integration

In our stateController.cs script, we need to add a new UnityEngine code library so that we can access the UI library functions. We also need to create a script Text variable for each UI-Text GameObject that will be linked to the UI Text in the game scene so that it can modify the properties of the UI-Text GameObject. We need to make sure our Text variables are declared as public instance variables so that they will show up in the script-component section of the inspector panel for the object that we've attached our script to.  

````java
using UnityEngine;
using UnityEngine.UI;  //make sure to add Unity UI library
using System.Collections;


public class NumberGame : MonoBehaviour {
	public Text gameText;  //game dialog
	public Text numberText;  //guessed number
	public Text instructionText;  //input instructions
	
	}
	
````

Next, go back into Unity, select the Canvas object in the Hierarchy Panel, (make sure we've attached our script to the Canvas GameObject), then we need to connect our TextUI elements with the public instance variables from our script component. We can do this by either dragging the UI-Text object from the hierarchy panel to the script component text-box, or we can select the text element in the script component, select the dot to the right of the text field and it will open a pop-up of all possible game-objects that we can select to connect to this script variable element.   [See Screenshot Animation 2 Below](https://kdoore.gitbooks.io/cs-2335/content/state_controlled_ui-text.html#animations)


###Connecting C# Text elements to Unity UI-Text.
In the NumberGame.cs file, we need to write code to modify the UI text elements.  We've declared the public Text elements as class instance variables.  Now in the code we want to modify the Text.

We can initialize the text elements in Start()
```
void Start(){
    gameText.text="Want to play a game? Y or N \n" ;
````
###Enumeration: GameStates

In C#, we can use Enumeration-Types to create custom data-types which function as named constants.  We use the C# keyword enum to declare our custom data-type, then we must initialize the values using a comma separated list of values.  
We'll define an enum to provide a set of gameStates to control our game's execution logic.
```java
    enum gameStates{ initialize, start, game, win, lose, end };
	gameStates activeState = gameStates.start;  //create and initialize a variable using our custom Enumeration-type
```

###NumberGame.cs 

Here is the code for the State-controlled version of the NumberGame project. 

It is important to realize that in the if-statement blocks, where we are checking to see if any valid input keys have been entered, these if-statement blocks are executed only for 1-brief instant of time, so we should not put code in these statement blocks that we expect to see displayed on the screen. We use these statement blocks to change the activeState, not to display any text since the keypress event is an instantaneous trigger.  
 

```
using UnityEngine;
using UnityEngine.UI;
using System.Collections;

public class StateController : MonoBehaviour {

	enum gameStates{ initialize, start, game, win, lose, end };
	gameStates activeState;
	
	public Text stateText;    // UI text to display current gameState
	public Text instructText; //put instruction text here
	
	// Use this for initialization
	void Start () {
	      stateText.text="Hello";  //initialize text
	      activeState=gameStates.initialize;  //initialize active state
	      Debug.Log ("gameStates.start " +  activeState);
	}
	
	// Update is called once per frame
	void Update () {
	
		switch(activeState){
		case gameStates.initialize: {
			stateText.text=activeState.ToString();
			instructText.text="Press G to begin game or Q to quit";
			if(Input.GetKeyDown(KeyCode.G)){
				activeState=gameStates.start;
			} 
			else if( Input.GetKeyDown (KeyCode.Q)){
				activeState=gameStates.end;
			}
			break;
		}
		case gameStates.start: {
			stateText.text=activeState.ToString();
			instructText.text="Pick a number, press Enter when ready,";
			if(Input.GetKeyDown(KeyCode.Return)){
				activeState=gameStates.game;  //change state
			} 
			break;
		}
		case gameStates.game: {
			stateText.text=activeState.ToString();
			instructText.text="Welcome to the game, press W to win";
			if(Input.GetKeyDown(KeyCode.W)){
				activeState=gameStates.win;  //change state
			} 
			break;
		}
		case gameStates.win: {
			stateText.text=activeState.ToString();
			instructText.text="You won";
			break;
		}
		default:
		{
			Debug.Log("default case");
			break;
		}
	}	//end switch
	
	
	}
}
```

### Animations 
1. How to Create Text-UI Game Objects and Attach Script to Canvas Object
2. How to Connect Text-UI Game Object to Script Text Element


![](GU6iOIPXxo.gif)

![](jfawLfwFA0.gif)


###Questions
1.  How can we add a counter to limit the number of computer attempts so that the player can also win the game?
