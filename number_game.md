# Number Game

We've all played a number-guessing game, where one player thinks of a number within in a given range of integers and the other player must guess the number in as few guesses as possible.  Let's look at how we can create a similar game using Unity.

### Console Game

For the first iteration of this game, we'll design the game to run in the console, where we have text prompts to accept user input. The player selects a single value within the given range of numbers and the computer tries to guess the player's secret number. The game-play mechanics are that the user selects/enters either the up-arrow or the down-arrow to indicate the number is higher or lower than the current guess.  If the computer selects/guesses the correct answer, then the user hits the return/enter key, and the game starts over.  To be an interesting game, the player should have an opportunity to win the game.

### Binary Search

We will write the program so that the computer makes the minimum number of guesses.  First, it is important to note that when selecting a number that falls within a specific range of numbers, we are talking about integer numbers and we are considering that they have their natural ordering:

* notice that we are searching a **sorted list** of values. 

At each step, the computer makes a guess that splits the number range in half by picking the value at the midpoint of the range. This guessed value corresponds to the  \(max + min\) / 2.  If the guessed, midpoint value does not match the player's selected number, then the computer asks the player whether their number is higher or lower than the current guessed value. Then we need to reset the endpoints of the search range so that the endpoints match this new, smaller range. If the user's number is higher than the computer's guess, then we need to set the new minimum of the new range to the current guessed value `min = guess`.  So, at each step in binary search, the number of possible values is divided in half, therefore, the maximum number of tries that the computer must make is   log  of the initial range plus one: $$log_2(n)+1$$.

> "Although the basic idea of binary search is comparatively straightforward, the details can be surprisingly tricky…"  
>     [Donald Knuth](https://en.wikipedia.org/wiki/Binary_search_algorithm#Number_guessing_game)

For an initial range of 0 to 64, the computer will make no more than 7 guesses since 2 to the 6th power is 64.

We walked through the logical design of the project in class by first creating a logic flow diagram, a simplified version is shown below.  In the diagram below we have identified the section of logic that must be repeated for each round of the game, that code will need to be executed using the Unity Update\(\) function, so we let unity Update\( \) function as a loop for our logic.

### Flow-Chart: Game Logic

![](flowChart.png)

Based on the flow diagram, we  can implemented a basic C\# class: NumberGame in monoDevelop and then attach the code to an empty gameObject in the default scene to create a console version of the game.

The flow-chart symbols can guide us in creating our code.

* Rectangle: initialization or computation with variables.
* Diamond: check \(input\) or compare values to determine branching logic, use: if, else, etc.
* Parallelogram:  Program output
* Ellipse: start or end

Below is a first pass at code for our console based game.

```C#
//Code Updated: Jan 19, 2016

using UnityEngine;
using System.Collections;

public class NumberGame1 : MonoBehaviour {

    public int min, max, guess;

    // Use this for initialization
    void Start () {
        min = 0;
        max = 64;
        guess = (min + max) / 2;
        Debug.Log("Do you want to play a Game, if so enter Y, else enter N");

    }

    // Update is called once per frame
    void Update () {
        if (Input.GetKeyDown (KeyCode.Y)) {
            Debug.Log ("Think of a number between " + min + " and " + max + " press Enter when ready");

        } 
        if (Input.GetKeyDown (KeyCode.N)) {
            Debug.Log("No game today");
        }
        if (Input.GetKeyDown(KeyCode.Return)){
            Debug.Log("Is your number " + guess + " If it matches, press C");
            Debug.Log("Is your number higher, the press up arrow" );
            Debug.Log("Is your number lower, the press down arrow" );
        }

        if (Input.GetKeyDown (KeyCode.UpArrow)) {
            min = guess;
            NextGuess ();
        }
        if (Input.GetKeyDown (KeyCode.DownArrow)) {
            max = guess;
            NextGuess ();
        }
        if (Input.GetKeyDown (KeyCode.C)) {  //correct value
            Debug.Log("The computer wins");
        }

    }  //end Update

    void NextGuess(){
        guess = (min + max) / 2;
        Debug.Log("Is your number " + guess + " If it matches, press C");
        Debug.Log("Is your number higher, the press up arrow" );
        Debug.Log("Is your number lower, the press down arrow" );
    }


}  // end of class
```

### Console Panel Settings:

In the Unity Console Panel, make sure to select tab settings: Collapse, Clear on Play.

![](Screenshot 2016-01-19 14.40.28.png)

### Questions

Can you extend this program:

* Use User-Interface Text GameObjects for player prompts?
* How can we add a counter to limit the number of computer attempts so that the player can also win the game?
* Check for `plus-one` type errors: does our method insure that the endpoints of the range can be selected as valid selections?  Is integer division adversely impacting our program?  If our range of numbers is 1 to 4, and we select number 4, what is the progression of values for: min, max, guess, and input

Example Program Values - Integer Truncation Problem

| Min | Max | Guess | Input |
| :--- | :--- | :--- | :--- |
| 1 | 4 | \( 1 + 4 \) / 2 = 2 | Higher |
| 2 | 4 | \( 2 + 4 \) / 2 = 3 | Higher |
| 3 | 4 | \( 3 + 4 \) / 2 = 3 | Higher |
| 3 | 4 | \( 3 + 4 \) / 2 = 3 | Higher |

As demonstrated above, the computer will never be able to select the value **4** as the correct guessed value.  How can we fix this?

