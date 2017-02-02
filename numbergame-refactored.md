#NumberGame Refactored

Below is a refactored version of NumberGame.cs where functions have been created to simplify code logic.

In the code below, there are 3 UI  Text components and a UI Button that have been integrated into the game.  Integration of the StartButton has required modification of the Switch Case logic for the case: GameState.Initialize.  When we use a button, then we no longer need logic in Update( ) to listen for keyboard input as possible events to change state.  Instead, that game-start logic has been put in the ButtonStartGame( ) function which gets executed when the button is clicked.  


```java
using UnityEngine;
using UnityEngine.UI;
using System.Collections;

public enum GameState
{
	Initialize,
	Start,
	Gameplay,
	Win,
	Lose
}

public class NumberGame : MonoBehaviour
{
	/// <summary>
	/// GameVariables that will change
	/// </summary>
	private int min, max, count, maxCount;
	public int guess;
	public GameState activeState;

	//game object component - object references
	private Text instructionText, promptText;
	private Text guessText;
	private Button startButton;


	//declare variables
	// Use this for initialization
	void Start ()
	{
		//set object references - cache 
		instructionText = GameObject.Find ("InstructionText").GetComponent< Text > ();
		promptText = GameObject.Find ("PromptText").GetComponent< Text > ();
		guessText = GameObject.Find ("GuessText").GetComponent< Text > ();

		startButton = GameObject.Find ("StartButton").GetComponent<Button> ();
		startButton.onClick.AddListener (ButtonStartGame);

		Debug.Log ("Do you want to play a game?");
		instructionText.text = "Do you want to play a game?";

		activeState = GameState.Initialize;

		InitializeGameVariables ();

	}
		
	// Update is called once per frame
	void Update ()
	{
		switch (activeState) {

		case GameState.Initialize:
			//Debug.Log ("In Initailize");
			break;

		case GameState.Start:
			if (Input.GetKeyDown (KeyCode.Return)) {
				activeState = GameState.Gameplay;
				NextGuess ();
			}
			break;

		case GameState.Gameplay:
			//Debug.Log ("In Gameplay");
			if (Input.GetKeyDown (KeyCode.Return)) {
				activeState = GameState.Win;

				instructionText.text = "I Win, you lose";
				InitializeGameVariables ();
			}

			if (Input.GetKeyDown (KeyCode.UpArrow)) {
				min = guess + 1;
				NextGuess ();
			}

			if (Input.GetKeyDown (KeyCode.DownArrow)) {
				max = guess - 1;
				NextGuess ();
			}

			break;

		case GameState.Win:
			//Debug.Log ("In Win");
			break;

		case GameState.Lose:
			//Debug.Log ("In Lose");
			break;

		default:
			Debug.Log ("No match - Default");
			break;
		
		}  //end of switch


	}
	//end of Update
	

	//TODO add comments describing function
	private void InitializeGameVariables ()
	{
		min = 0;   // initialize variables
		max = 100;
		count = 0;
		maxCount = 5;

		startButton.gameObject.SetActive (true);

		promptText.text = "";
		guessText.text = "";
	}
	
	//TODO add comments describing function
	private void NextGuess ()
	{
		// count logic has been removed
		//add logic to increment the count variable
		//add logic to test whether count > maxCount
		//use else statement for other code - so it doesn't get
		//executed if lose test is true
		//if lose test is true - the following code should be executed
			//Debug.Log ("Max count reached - Computer Loses");
			//activeState = GameState.Lose;
			//instructionText.text = "You Win, Computer Loses";
			//InitializeGameVariables ();

		guess = (min + max) / 2;
		instructionText.text = "Is your number";
		guessText.text = guess.ToString ();
		promptText.text = "If correct, press Enter, if your number is higher, press Up Arrow, if lower press Down Arrow";
		Debug.Log ("Is your number" + guess);
			


	}
	
	//TODO add comments describing function
	public void ButtonStartGame ()
	{
		activeState = GameState.Start;

		string s = string.Format ("Pick a number between {0} and {1}  ", min, max);
		Debug.Log (s);
		instructionText.text = s;

		Debug.Log ("Press enter when ready");
		promptText.text = "Press enter when ready";
		guessText.text = "";

		startButton.gameObject.SetActive (false);
	}
	
}//end of class


```