###EndScene / EndState

When the MiniGame has ended, you may choose to load the EndScene and show a panel with text that tells about your game results.  This code can be added to EndState.cs file.

Here are some code snippets that might help you determine logic to be used in the final scene.  You'll need to create a UI Panel and UI text to update depending on the GameData Results.



```java
//updated 4/12/18 3:40pm
	
public void InitializeObjectRefs ()
	{
	//other code used in EndState, not shown
    
    	
        gameOverText = GameObject.Find("GameOverText").GetComponent<Text>();
       
        int health = GameData.instanceRef.Health;
        int score = GameData.instanceRef.TotalScore;
        if( health <= 0){
            gameOverText.text = "Sorry. You lost the game";
        }else if(score > 0 ){
            gameOverText.text = "You won the game, Your Score is: " + score;
        }else{
            gameOverText.text = "Go back to the start and play the game ";
       
        }
	}

```





###Restart the Game /  MiniGameState