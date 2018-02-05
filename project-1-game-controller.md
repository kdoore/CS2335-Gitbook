#Project 1 - GameController Class
It's common to have a GameController class to manage the higher level logic of the game, such as keeping track of score, winning and losing, etc.  We'll create a GameController class for this purpose, and we can attach the script to an empty gameObject called GameController.  

###What GameObject should have the GameController Script Attached?
When deciding what gameObject to attach a script to, we need to consider all gameObjects that will be managed by a script, such that, a script should be placed on a gameObject that is conceptually broader than any single gameObject manipulated by the script.  Therefore, this script could optionally be on the Main Camera, or an Empty GameObject created for the purpose of holding the script, or potentially the Canvas GameObject.  If we put this script on the Canvas, that would imply that it's only controlling UI-elements.  However, in this case, the GameController script will also be storing the score and causing the first Apple to be dropped from the AppleTree.  For this reason, the Canvas might not be the best gameObject for this script to be attached to.  So, we have decided to put it on an empty gameObject called GameController, where we should move this gameObject higher in the Hierarchy panel, so it's just below the camera, this will make it easier to notice when looking at the Hierarchy panel.

###Create Script: GameController.cs and GameController GameObjects
1.  Create a new script, name it:  GameController
2.  Create an Empty GameObject, name it: GameController
3.  Move this GameObject near the top of the Hierarchy Panel, just below the Main Camera
4.  Add the GameController.cs script to this GameController empty gameObject.

    ![](/assets/Screen Shot 2018-02-01 at 1.37.42 PM.png)       ![](/assets/Screen Shot 2018-02-01 at 1.41.39 PM.png)
[Link to Animation showing how to complete the steps above:
](http://recordit.co/PoSc8aALqU)

###UI Elements - using UnityEngine.UI
As discussed in the previous page, we will have added several User Interface (UI) elements to the game.
The code below shows how we'll interact with those elements from the GameController.cs script.  
It's critical that we add a directive to the top of our script to include the UnityEngine UI Library, and that we add this to any script that modifies any UI Components:

```java
 using System.Collections;
 using System.Collections.Generic;
 using UnityEngine;
 using UnityEngine.UI;   //Add this additional directive for UI components
```

#Object Reference Variables for GameObject-Components: ScoreText, GameOverText

###Declare, Initialize, and Modify
 **Declare:** At the top of the class file definition, we will **declare object-reference variables** to allow us to interact with Components on these UI GameObjects. 

  - Declare our object-reference variables to the Text Component we want to control via script, at the top of the class definition.

```java
    private Text scoreText, gameOverText; //dataType is Text (Component) 
```

 **Initialize:** We need to **initialize the object reference variables**, we do that in the Start() function.  This is a 2 step process, first we need to find the gameObjects, then we specify the dataType < T > of Component we will be accessing. (< T > is a `C# Generics` placeholder where we must replace **T** with the corresponding component dataType, here it's a < `Text` > component). 

    - To initialize the object-reference variables, we first need to find the gameObject, then find the Text component on the given gameObjects, this will be done in Start().

```java
 void Start(){
    scoreText = GameObject.Find("ScoreText").GetComponent<Text>();
    gameOverText = GameObject.Find("GameOverText").GetComponent<Text>();
   
     //other code in Start(), not shown here 
    }
```
**Modify:** We'll be modifying the Text GameObjects, specifically, we'll modify the `text` field of the Text Component, which must be formatted as a String in our code. This code will be located along with other code in the  
  
    ```java
    //This code shows how to set the text field of the scoreText Text Component, we're setting a string value by using quotes, then we use + score to concatenate the current value of the score variable so it's displayed after the word "Score: "
     
     public void StartGame( ){
     score = 0;
     scoreText.text = "Score: " + score;
     
     //other code in StartGame(), not shown here  
     }
     ```
    
#Object Reference Variables for GameObjects: StartButton, GameOverPanel
When working with GameObjects in our code, often we want to toggle visibility, to show / hide a gameObject.  There are several ways show / hide UI gameObjects, we'll look at other methods later in the course.  If we will hide a gameObject by setting 'setActive( false )', **the gameObject must be active when the scene first starts**, otherwise, we won't be able to make our initial connection to find the gameObject. 

###Declare, Initialize, Modify

**Declare:** At the top of our class definition, we declare Object Reference variables that will refer to GameObjects we want to control in our script.
    
```java
private GameObject startButton, gameOverPanel;

```
**Initialize:**   In Start(), we need to find 2 gameObjects by name, a UI-Panel, and UI-Button, once we've initialized this connection between our object-reference variable and the GameObject in the scene, then we can use this variable to toggle the value of setActive( true / false ), throughout the lifetime of this script and scene.

```java

void Start(){
    startButton = GameObject.Find("StartButton");
    gameOverPanel = GameObject.Find("GameOverPanel"); 
 
 //other code in Start() not shown here
}

```
**Modify:**  Finally, we can modify these GameObjects that we have connected with, via the Object-Reference variables.  Often, when working with GameObjects, we'll be toggling the gameObject so that it's active / not active in the game scene.  We will be hiding these GameObjects by setting the `SetActive(true /false ) `method to true or false.  The StartButton will be modified in the StartGame( ) method we'll write later in the page.

```java

void Start(){
    startButton = GameObject.Find("StartButton");
    gameOverPanel = GameObject.Find("GameOverPanel"); 
 
    gameOverPanel.setActive( false ); //disables / hides the GameObject
}

```



###Game-Control Logic
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
        gameOverPanel.SetActive(false); //Hide gameOverPanel and gameOver text
        gameActive = false; //set to true when StartButton is clicked
	}  //end Start()
```

###StartGame Custom Method and StartButton Function
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

        appleTree.DropObjects(); //start objects dropping

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

