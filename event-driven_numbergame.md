# Event-Driven Number Game

In the previous iterations of the game, we have used the Update() Unity Event function to implement code for control and logic of the game.  In Update() for every frame execution, our code checks the current activeState, then checks for any input that is valid for that state, then we reset the UI-Text elements to the correct values when the activeState changes.  In effect, the only time that we need to change UI-Text element values is when an input event has occurred. 

We can restructure our game so that we use UI-Buttons and the Unity managed: button `on-click` event, then we can eliminate some of the code that's being executed, as we check for valid keyboard input values each frame in the Update function.  

###UI-Button: OnClick Event-Handlers
Unity provides an event system for the Unity User-Interface (UI) components. GameObjects like Button have a Button Component that provides easy configuration to have our custom functions executed when the user clicks on a UI button in our active game scene. 

###Custom Function
In order to add custom behavior to a UI-Button, first we need to write some code that we'd like to have executed when the button has been clicked.  For our number game project, we can add a start game button that the user will click to indicate that they want to play the game.  Currently, we prompt the user when they are in the Initialize GameState to enter 'Y' if they went to play, or to enter 'N' to quit.  So, we have the following logic in our code, where we are listening for 'Y' when the activeState = GameState.Initialize:
```C#
	 void Start () {
        min = 0;  //initialize values
        max = 64;
        guess = (min + max) / 2;
        activeState = GameState.Initialize;
        Debug.Log("Do you want to play a Game, if so enter Y, else enter N");
        gameText.text = "Do you want to play a Game, if so enter Y, else enter N?";  //ui text prompt
    }
	
	void Update(){
	switch(activeState) {
			
	case GameState.Initialize:
	
	///this Initialize code will be moved into a function to be executed when the button onClick event happens
	 
			if (Input.GetKeyDown (KeyCode.Y)) {
				Debug.Log ("Think of a number between " + min + " and " + max + " press Enter when ready");
				activeState = GameState.Start;
			} 
	//this code won't be used when we implement the start button
	
			if (Input.GetKeyDown (KeyCode.N)) {
				Debug.Log ("No game today");
				activeState = GameState.End;
			}

		break;
		
		case GameState.Start:
		     //code here
		break;
		
		
		case GameState.GamePlay:
		    //code here
		break;
		
		///other code
	} //end Update()

```

So, we need to refactor our program and move code out of the Update function.  We also need to change the prompt in the Start function so the user knows to press the button to start the game.

```C#
gameText.text = "Do you want to play a Game, if so press the Start Game button, else enter N?"; 


//remove this code from update since we're no longer testing for input of 'Y' during gameState.Initialize

if (Input.GetKeyDown (KeyCode.Y)) {
				Debug.Log ("Think of a number between " + min + " and " + max + " press Enter when ready");
				activeState = GameState.Start;
			} 

```
We move the inner code into a new public function: StartGame()
```C#
public void StartGame(){    //this must be a public function
		activeState = GameState.Start;
		Debug.Log ("Think of a number between " + min + " and " + max + " press Enter when ready");
		promptText.text = string.Format ("Think of a number between {0} and {1} \n press Enter when ready", min, max);
	}

```
In Unity, we need to add a UI-Button GameObject to the scene.  We'll want to set custom values for highlight and normal colors so we can verify the button responds to mouse interaction when we hover over it during game-play mode.

Then we need to add our custom function as a OnClick( ) event handler for the button. 
To do this, we need to connect our object reference in our code to the gameObject's component in the scene.  We do this in code in the Start() function

```C#
using UnityEngine.UI;

///declare our variable at top of class file
private Button startButton;

void Start()
    {
    ///other code
    
    startButton = GameObject.Find("StartButton").GetComponent<Button>();
    startButton.onClick.AddListener(StartGame);  //StartGame is our custom function that we want executed when the startButton is clicked
    }
```

It's important to make sure we've declared the custom function, StartGame, as public, so it can be executed by Unity. Once we select the correct function, our button should execute this fuction when clicked.  


###Hide Button after Use

Then in our code: Once the button has been clicked, and we've executed our StartGame tasks, then we can set the StartButton variable to inactive by finding the gameObject that is connected to the Button component we have been working with: 

```C#
public void StartGame(){
		activeState = GameState.Start;
		Debug.Log ("Think of a number between " + min + " and " + max + " press Enter when ready");
		promptText.text = string.Format ("Think of a number between {0} and {1} \n press Enter when ready", min, max);
		
        startButton.gameObject.SetActive (false);  //This in-activates the gameObject that the button component is attached to.
	}
	
```	
