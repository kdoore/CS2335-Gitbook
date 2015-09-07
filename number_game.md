#Number Game
We've all played a number-guessing game, where one player thinks of a number in a range and the other player must guess the number in as few guesses as possible.  Let's look at how we can create a similar game using Unity.

###Console Game
For the first iteration of this game, we'll design the game to run in the console, where we have text prompts to accept user input. The computer makes guesses and the user selects either the up-arrow, down-arrow to indicate the number is higher or lower than the current guess.  If the computer selects the correct answer, then the user hits the return key, and the game starts over.  

##Binary Search
We will write the program so that the computer makes the minimum number of guesses necessary.  At each step, the computer makes a guess that splits the number range in half.  This corresponds to the  (max + min) / 2.  If the guess does not match, then we need to reset the endpoints of the range, if the user's number is higher than the computer's guess, then we need to set the minimum of the range to the current guess.  So, at each step in binary search, the number of possible values is divided in half, therefore, the maximum number of tries that the computer must make is not more than log - base 2 of the initial range.  For an initial range of 32, the computer will make no more than 5 guesses since 2 to the 5th power is 32.  

We walked through the logical design of the project code in class by first creating a logic flow diagram.  Then we implemented the code in C# in monoDevelop and we attached the code to the main-camera in the default scene.  

```
using System.Collections;

public class NumberGame : MonoBehaviour {
		int min, max, guess;
		
	// Use this for initialization
	void Start () {
		StartGame();
		}
	
	
	// Update is called once per frame
	void Update () {
		if(Input.GetKeyDown(KeyCode.UpArrow)){  //guess is too low
			min=guess;
			NextGuess();
		}
		else if(Input.GetKeyDown(KeyCode.DownArrow)){  //guess is to high
			max=guess;
			NextGuess();
		}
		else if(Input.GetKeyDown(KeyCode.Return)){ //computer wins!
			print("I won, I'm brilliant");
			StartGame();  //restart game
		}
			 
	}
	
	void StartGame(){  //initialization of values and game prompts
		min=0;
		max=100;
		guess=(min+max)/2;  ///initial guess - binary search algorithm
		print("Do you want to play a game?");
		print("Think of a number between " + min + " and " + max);
		print("Is the number higher or lower than " + guess + " ?");
		print("'up-arrow' for higher, 'down-arrow' for lower, 'enter' for equal");
	}
	
	//Calculate next guess and generate next game prompts
	void NextGuess(){
		guess=(min+max)/2;
		print("Is the number higher or lower than " + guess + " ?");
		print("'up-arrow' for higher, 'down-arrow' for lower, 'enter' for equal");
	}
}
```

