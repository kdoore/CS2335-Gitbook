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
To allow players to restart the game, you might want to add this code to MiniGameState.cs script.  This is necessary because the data stored in GameData is not destroyed when switching scenes, this is good when wanting to access GameData information in other scenes, but it's a problem when wanting to restart the MiniGame, so the MiniGame state is a good place to call to  ResetGameData( ) . 	
	

```java
   //updated 4/12/18 3:40pm
       
       //CODE in MiniGameState.cs
	public void InitializeObjectRefs ()
	{
        GameData.instanceRef.ResetGameData();
       }
       
       
       ////CODE IN GameData
       //call when scene is reloaded in MiniGameState.cs
     public void ResetGameData(){
          health = 100;
          levelScore = 0;
          totalScore = 0;
          inventory.Clear();
     }
```

