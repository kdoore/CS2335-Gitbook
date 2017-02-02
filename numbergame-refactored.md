#NumberGame Refactored

Below is a refactored version of NumberGame.cs where functions have been created to simplify code logic.

In the code below, there are 3 UI  Text components and a UI Button that have been integrated into the game.  Integration of the StartButton has required modification of the Switch Case logic for the case: GameState.Initialize.  When we use a button, then we no longer need logic in Update( ) to listen for keyboard input as possible events to change state.  Instead, logic has been put in the ButtonStartGame( ) function which gets executed when the button is clicked.  


```java

using UnityEngine;
using UnityEngine.UI;
using System.Collections;

public enum GameState
{
	Initialize,
	Start,
	GamePlay,
	Win,
	Lose,
	End
}

public class NumberGame : MonoBehaviour
{
	///DONT INITIALIZE Variables here HERE
	private int min, max;
	private int count, maxCount;  //use for count logic so player can win the game
	public int guess;
	public GameState activeState;

	//Component References
	private Text instructionText, promptText;
	private Text guessText;
	private Button startButton;

	/// Use Unity Start function for initialization of object reference variables 
	///Object References for Unity Components are matched to game scene gameObject components
	void Start ()
	{
		// find all gameObjects and components - one time - in Start
		instructionText = GameObject.Find ("InstructionText").GetComponent< Text > ();
		promptText = GameObject.Find ("PromptText").GetComponent< Text > ();
		guessText = GameObject.Find ("GuessText").GetComponent< Text > ();

		startButton = GameObject.Find ("StartButton").GetComponent< Button > ();
		startButton.onClick.AddListener (ButtonStartGame);

		// set activeState
		activeState = GameState.Initialize;

		instructionText.text = "Do you want to play a game?";
		promptText.text = "Press The Start Button to Play";
		guessText.text = "";

		Debug.Log ("Do you want to play a game?");
		Debug.Log ("Press button to play");

		InitializeGameVariables ();
	}

	/// <summary>
	/// called at the start of each new game
	/// initializes game variables, sets startButton to active
	/// </summary>
	void InitializeGameVariables ()
	{
		max = 100;
		min = 0;
		count = 0;
		maxCount = 5;

		//guess = (min + max) / 2;  move this logic to NextGuess function
		startButton.gameObject.SetActive (true);
	}

///This method is executed when the StartButton is clicked
///It is added as a Listener to the button's OnClick event handler
	public void ButtonStartGame ()
	{
		Debug.Log ("Button was clicked");

		//change state
		activeState = GameState.Start;

		string s = string.Format ("Think of a number between {0} and  {1}  ", min, max);
		Debug.Log (s);
		instructionText.text = s;

		string p = "Press enter when ready to play";
		Debug.Log (p);
		promptText.text = p;

		startButton.gameObject.SetActive (false);  //hide button

	}
	
	// Unity Update( ) is called once per frame
	void Update ()
	{
		switch (activeState) {
			
		case  GameState.Initialize:

			//code moved to ButtonStartGame - input event is ButtonStartGame
			break;

		case GameState.Start:

			if (Input.GetKeyDown (KeyCode.Return)) {
				activeState = GameState.GamePlay;
				NextGuess ();// call initial time with min = 0, max = 100
			}
			break;

		case GameState.GamePlay:

			if (Input.GetKeyDown (KeyCode.Return)) {
				activeState = GameState.Win;
				instructionText.text = "Computer Wins";
				ResetGame ();
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
			Debug.Log ("Computer Wins");
			break;

		case GameState.Lose:
			Debug.Log ("Computer Loses");
			break;

		default:
			Debug.Log ("There are no matching states");
			break;
		}//end of the switch statement
				 
	}//End of update
	
	
	/// <summary>
	/// Called once at the beginning of Gameplay, 
	/// then Called repeatedly in Gameplay when up or down arrow is selected
	/// </summary>
	private void NextGuess ()
	{
		///add count logic here - to test for lose event
		/// the code below should only be executed if 
		/// lose condition hasn't happened yet

			guess = (min + max) / 2;
			instructionText.text = "Is your number: ";
			guessText.text = guess.ToString ();
			promptText.text = " If correct press enter, if your number is lower, press down arrow, if your number is higher press up arrow";

			Debug.Log ("Is your number " + guess);
			Debug.Log ("If correct press enter, if your number is lower, press down arrow, if your number is higher press up arrow");

	}

	/// <summary>
	/// Resets the game. This is called when the game is won or lost
	/// allows for restarting the game
	/// </summary>
	private void ResetGame ()
	{
		promptText.text = "Press The Start Button to Play Again";
		guessText.text = "";
		startButton.gameObject.SetActive (true);
		InitializeGameVariables ();
	}

}
//end of class


```