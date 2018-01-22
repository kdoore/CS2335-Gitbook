###Project 1 - GameController Class
It's common to have a GameController class to manage the higher level logic of the game, such as keeping track of score, winning and losing, etc.  We'll create a GameController class for this purpose, and we can attach the script to an empty gameObject called GameController.

This class will manage: 
    * Starting the game when the StartButton is pressed
    * Updating the Score and Score Display when items hit the Basket
    * Determining if the game is over and if the player has won or lost the game
    * Displaying / Hiding the GameOver message
    * Showing / Hiding the StartButton 
    
    
###Declare Variables

```java
using System.Collections;
using System.Collections.Generic;
using UnityEngine;
using UnityEngine.UI;  //Add to allow script control of UI elements

public class GameController : MonoBehaviour {

    private int score=0;  
    private int winScore = 20;  
    
    public bool gameActive = true; //control activation of gameObjects, like dropping apples

    //text elements that will be modified during game
    private Text scoreText, gameOverText;

    //game objects that will be set active / inactive
    private GameObject  startButton, gameOverPanel;

    //reference to the appleTree script component on the AppleTree gameObject
    private AppleTree appleTree;
```

###Start Event Function
The Unity Start event function is used to initialize variables, find gameObjects and components to interact with during the game:

```java
// Use this for initialization
	void Start () {
        //find all gameObjects that will be activated / deactivated 
        startButton = GameObject.Find("StartButton");
        gameOverPanel = GameObject.Find("GameOverPanel");

        //get a reference to the AppleTree script component
        appleTree = GameObject.Find("AppleTree").GetComponent<AppleTree>();

        //find text components for score and gameOver panel
        scoreText = GameObject.Find("ScoreText").GetComponent<Text>();
        gameOverText = GameObject.Find("GameOverText").GetComponent<Text>();

        //deactivate necessary gameObjects
        gameOverPanel.SetActive(false);
        gameActive = true;
	}
```

###StartGame and StartButton
The StartGame function will be executed when the StartButton is clicked.  This function must be public so it can be execute.

```java

    /// <summary>
    /// Starts the game.
    /// this function Must be public 
    /// function to be called by StartButton, 
    /// This will be added to the OnClick event in the inspector for the StartButton
    /// </summary>
    public void StartGame()
    {
        score = 0;  //reset score to 0
        scoreText.text = "Score: " + score;

        gameActive = true; //this is used in other classes to control gameObjects

        appleTree.DropApple(); //start apples dropping

        startButton.SetActive(false); //hide StartButton
        gameOverPanel.SetActive(false); //hide gameOver panel

    }
```
The image below shows that the StartButton's Button script has been configured to show both a Normal and Highlighted color, this is important for debugging, it will allow us to verify that the button is responding to mouse-over events.

The OnClick section of the Button script shows that the GameController object has been selected, this allows us to select from the public methods of this gameObject's script components.  We have selected the GameController script component's StartGame( ) method.  This method will be executed when the StartButton is clicked. 

![](/assets/Screen Shot 2018-01-20 at 7.23.59 PM.png)

###UpdateScore Method
The UpdateScore method is called from the Basket class whenever an object that's either an Apple or a Rock collides with the Basket. The points passed into this method can either add to the score or subtract from the current score.  Each time, the score is checked to see if the player has won or lost, in which case the GameOver text is set to give the player feedback on the outcome of the game.  The StopGame() method is called if the game is over, it will manage gameOver logic. 


```java

    //Update gamescore by points passed into function, called from Basket class
    public void UpdateScore(int points)
    {
        score += points; //update score
        scoreText.text = "Score: " + score; //update displayed score
      //determine if the game is over and set the text to be displayed 
     if (score >= winScore) 
        {
            gameOverText.text = "You are a Winner!";
            StopGame();
        }
        else if (score <= 0)
        {
            gameOverText.text = "Sorry, you lost this time.";
            StopGame();
        }
    }
```

###StopGame Method
If the game is over, the StopGame method is called from UpdateScore, it activates the StartButton and the GameOverPanel.  Setting gameActive to false will allow the AppleTree to stop dropping apples, it'll stop moving, and the Basket will also stop moving.  

```java

//Show gameOverPanel, startButton
//set appleTree.gameActive to false to stop apples from dropping
    private void StopGame(){
        gameActive = false;
        gameOverPanel.SetActive(true);
        startButton.SetActive(true);
    }
	
```

