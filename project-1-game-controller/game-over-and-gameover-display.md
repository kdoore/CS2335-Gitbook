#Game-Over and GameOver Display

We need to define conditions that allow a player to win or lose the game.  The obvious place to put this code is in the GameController class, since that is also where we have a variable to keep track of the score and where we have the UpdateScore( ) method that changes the score when a basket collision has occurred.  

###Game-Over Event - UpdateScore( )

The score will determine if a player wins or loses, so we know that change in gameState will occur when the score has just been updated, so the UpdateScore( ) method a reasonable place to put this game-over logic. Before looking at the UpdateScore( ) method, we need to add 2 UI-GameObjects to the scene: A UI-Panel that has a child: UI-Text gameObject. These are used to display the score.  

###UI GameObjects: GameOverText, GameOverPanel
Just as we added the ScorePanel and the ScoreText, we need to add these gameObjects to the scene. Follow the animation below to add these objects, then we need to create object references to modify these in the GameController.cs script. 

###Animation - Add GameOverPanel, GameOverText

![](http://g.recordit.co/scE8CnkjS2.gif)

###GameController.cs Create Object References

** Declare **Object References

```java
 private Text gameOverText; //Text component
 private GameObject  gameOverPanel; //GameObject
```
**Initialize** to connect with GameObject / Components in Start()

```java

void Start () {
    //Other code in Start is not shown here
   //connect object reference variable with gameObject in scene    
   gameOverPanel = GameObject.Find("GameOverPanel");
   //connect object reference variable with Text component on gameOjbect in scene
   gameOverText = GameObject.Find("GameOverText").GetComponent<Text>();

   //deactivate the gameOverPanel, should be hidden at start
   gameOverPanel.SetActive(false);
     
    //Other code in Start is not shown here
	}

```
###GameController.cs UpdateScore( ) and StopGame( ) Methods
**Modify** - Set GameOverText when the game has ended, then make the panel and text visible.  This happens in UpdateScore( ).  We've also created a new method: StopGame(), this is where we show the GameOverPanel


```java

 //GameController.cs
    public void UpdateScore(int points)
    { 
        score += points;
        scoreText.text = "Score: " + score;
       
        //ADD CODE For Win condition
        if (score >= winScore)
        {
            gameOverText.text = "You are a Winner!"; //Set text to show win state
            StopGame();
        }
        else if (score <= 0)
        {
            gameOverText.text = "Sorry, you lost this time.";//Set text to show win state
            StopGame();
        }
    }
    
    
    //Show gameOverPanel, startButton
    //set appleTree.gameActive to false - this is used to control behavior of appleTree and basket.
    private void StopGame(){
        gameActive = false;
        gameOverPanel.SetActive(true);  //show gameOverPanel
        startButton.SetActive(true);   //show start button
    }



 public void StartGame()
    {
        //Add this code if you haven't already done this
        gameActive = true; //this is used in other classes to control gameObjects

       ///ADD THIS CODE to StartGame to hide this panel 
       gameOverPanel.SetActive(false); //hide gameOver panel
       
       ///other StartGame code not shown
    }
	

```
