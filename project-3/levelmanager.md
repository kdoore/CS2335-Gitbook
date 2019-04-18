#Level Manager

###GameData - check for the following updates:
You may need to add some new code to GameData, this is specified in the LevelManager-Events documentation.  

These are changes that should be incorporated into GameData prior to working with the Level Manager
    - levelScore variable, LevelScore property.
    - onPlayerDataUpdate, UnityEvent
    - inventory - Inventory scriptableObject
    - lives, Lives properties?
    
- Data-Events: These trigger onPlayerDataUpdate
    - Event: Add( )
        - increment levelScore with points
        - invoke onPlayerDataUpdate
    - Event: TakeDamage( )
        - invoke onPlayerDataUpdate
    - Event: AddItem( )
        - invoke onPlayerDataUpdate

###LevelManager Code 
You will need to modify this code to match your game's logic


```java
//last updated 4/18/19 


public class LevelManager : MonoBehaviour
{

    public enum LevelState
    {
        start,
        level1,
        level2,
        level3,
    }

    LevelState curLevel;   // FSM - 1 unit of memory

    int maxLevelScore = 30; //when to change levels, set in inspector

    //UI game Objects - LevelValue, StartGameButton, StartGamePanel
    public Button startGameButton;
    public CanvasGroup cg;
    public Text levelText;

    //references to custom script components
    public Spawner spawner1, spawner2, spawner3;  //add more spawners here

    //GameObject pickupParent1, pickupParent2, pickupParent3;


    void Start()
    {
        curLevel = LevelState.start;

        //comment out UI elements below if not using a start-screen / start-button
          startGameButton.onClick.AddListener(NextLevel);

        //make sure Start Panel is showing at start of Scene.
        Utility.ShowCG(cg);

         ///Update Check to see if level is over when playerDataUpdate event happens
        GameData.instanceRef.onPlayerDataUpdate.AddListener(CheckLevelEnd);

        //NextLevel(); //uncomment and use this option if not using StartGamePanel and StartGameButton to start the gameplay.
    }

    /// <summary>
    /// Checks the level end.
    /// Checks the data to see if the level has ended
    /// </summary>
    public void CheckLevelEnd()
    {
        int levelScore = GameData.instanceRef.LevelScore;
        Debug.Log("Check if level is over " + levelScore);

        //Add code here to see if health is <=0


        //if health is ok, then check if level is over
        if (levelScore >= maxLevelScore)
        { ///level has changed
          ///reset levelScore
            GameData.instanceRef.LevelScore = 0;   //reset GameData.LevelScore
            NextLevel();  //go to next level - call FSM
        }

    }

    //This method implements the Finite State Machine to Manage Level Logic. 
    //You will modify this code to correspond to your game's logic
    //This method is always called when an event has occured to end the level
    //Event types:  data-centric: Score > LevelScore, health <= 0, Player falls, 
   
    public void NextLevel()
    {

        switch (curLevel)  //check the curLevel, find matching case below
        {

            case LevelState.start:  // called when StartPanel, StartGameButton is clicked
                curLevel = LevelState.level1; //change level
                loadLevel1();
                break;

            case LevelState.level1:  //called when in Level1 from checkLevelEnd( ) 
                curLevel = LevelState.level2; //change level
                loadLevel2();
                break;

            case LevelState.level2: //called when in Level2 from checkLevelEnd( ) 
                curLevel = LevelState.level3; //change level
                loadLevel3();
                break;

            case LevelState.level3: //called when in Level3 from checkLevelEnd( ) 
                miniGameOver();
                break;

            default:
                Debug.Log("No match on curLevel");
                break;
        }
    }

    ////YOU WILL MODIFY THESE METHODS SO THEY CORRESPOND TO YOUR GAME"S LOGIC
    void loadLevel1()
    {
        //STARTS Gameplay, Spawner, etc

        Utility.HideCG(cg); //hide the StartGamePanel and StartGameButton
        spawner1.gameObject.SetActive(true);
        spawner1.StartSpawning();  //Make sure to remove this code from Start in the spawner script
        levelText.text = "Level 1";
    }

    void loadLevel2()
    {
        ///change background image?
        ///stop spawning?
        spawner1.activeSpawning = false;///stops spawning
        spawner1.DestroyAllPickups();
        ///change objects getting spawned?
        ///spawner2.StartSpawning();    //start next spawner 
        levelText.text = "Level 2";
    }

    void loadLevel3()
    {
        ///change background image?
        ///change objects getting spawned?
        ///stop spawning?
        //spawner2.activeSpawning = false;///stops spawning
        //spawner2.DestroyAllPickups();
        ///change objects getting spawned?
        ///spawner3.StartSpawning(); //start next spawner
        levelText.text = "Level 3";
    }

    //What happens when the MiniGame is over?  
    //How does a player leave the MiniGameScene?
    void miniGameOver()
    {
        GameData.instanceRef.LevelScore = 0;
        //invoke custom event to notify MiniGState where sceneChange logic an be executed.  
    }

}// end class

```

