###LevelManager - Final

_updated 4/26/2019_ _(Small changes to LoadLevel2, StartLevel2 to improve Level transition with ScreenFader.  Player is disabled, Level2Objects aren't loaded until Button is pressed )
_
Assumes you are using:  (If you choose not to use these, remove any code associated with these script components)
- CameraFollow Script
- ScreenFader Script

**Includes:**
- **timer** method - reloadTimer( timeInSeconds )
- **GameObject[] gameObjectLayers** - array of Level-specific Parent GameObjects for each level's features
- **Event Listeners:**
    - **GameData: onPlayerDataUpdate**  - Listener: CheckLevelEnd( )
    - **PlayerController: onPlayerDied** - Listener: ReloadMiniGame( )
    - **PlayerController: onPlayerReachExit** - Listener: NextLevel( )
    
- **Event Publisher** **OnMiniGameEnd**  - Listener: MiniGState
 
 You must add your custom code for Level3
 You must modify MiniGState, and EndState to include logic for consequences for the miniGame.  

```java

using UnityEngine;
using UnityEngine.UI;
using UnityEngine.Events;
using System.Collections;
using UnityEngine.SceneManagement;

public class LevelManager : MonoBehaviour
{

    public enum LevelState
    {
        start,
        level1,
        level2,
        level3,
        lose, 
        win
    }

    public UnityEvent onMiniGameEnd = new UnityEvent();  //broadcast to MiniGState

    LevelState curLevel; // FSM - 1 unit of memory

    public int maxLevelScore = 30; //when to change levels, set in inspector

    //UI game Objects - LevelValue, StartGameButton, StartGamePanel
    public Button startGameButton;
    public CanvasGroup cg; //if using an ResultsPanel
    public Text levelText;
    public Text timerText;
    Text btnText;
    public Transform playerSpawnPoint;
    GameObject player;
    PlayerController playerController;
    ScreenFader fader;
    CameraFollow cameraFollow;
    //references to custom script components
    public GameObject[] gameObjectLayers;
   
    void Start()
    {
        playerController = FindObjectOfType<PlayerController>();
        player = playerController.gameObject;
        playerController.onPlayerDied.AddListener(ReloadMiniGame);
        playerController.onPlayerReachExit.AddListener(NextLevel);
        GameData.instanceRef.onPlayerDataUpdate.AddListener(CheckLevelEnd);

        fader = FindObjectOfType<ScreenFader>();
        cameraFollow = FindObjectOfType<CameraFollow>();

        startGameButton.onClick.AddListener(NextLevel);
        btnText = startGameButton.GetComponentInChildren<Text>();

        gameObjectLayers[0].SetActive(true); //Level1 gameObjects, spawner, background, floor
        gameObjectLayers[1].SetActive(false); //Level1 gameObjects, spawner, background, floor
        gameObjectLayers[2].SetActive(false); //Level1 gameObjects, spawner, background, floor
        curLevel = LevelState.start;

        //show Start Panel at start of Scene.
        if (cg != null)
        {
            Utility.ShowCG(cg);
        }
       
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
        if (GameData.instanceRef.Health <= 0)
        {
            curLevel = LevelState.lose;
            GameData.instanceRef.miniGameWinner = false;
            MiniGameOver();
        }
        else if (levelScore >= maxLevelScore)
        { ///level has changed
          ///reset levelScore
            GameData.instanceRef.LevelScore = 0; //reset GameData.LevelScore
            NextLevel(); //go to next level - call FSM
        }

    }//end CheckLevelEnd

  //This method implements the Finite State Machine to Manage Level Logic.
    //You will modify this code to correspond to your game's logic
    //This method is always called when an event has occured to end the level
    //Event types: Score > LevelScore, health <= 0, Player falls, Player reaches exit
    public void NextLevel()
    {
        switch (curLevel) //check the curLevel, find matching case below
        {

            case LevelState.start: // called when StartPanel, StartGameButton is clicked
                curLevel = LevelState.level1; //change level
                StartLevel1();
                break;

            case LevelState.level1: //called when in Level1 from checkLevelEnd( )
                curLevel = LevelState.level2; //change level
                LoadLevel2();
                break;

            case LevelState.level2: //called when in Level2 from checkLevelEnd( )
                curLevel = LevelState.level3; //change level
                LoadLevel3();
                break;

            case LevelState.level3: //called when in Level3 from checkLevelEnd( )
                curLevel = LevelState.win;
                MiniGameOver();
                break;

            default:
                Debug.Log("No match on curLevel");
                break;
        } //end switch-case
        
    }//end NextLevel

    ////YOU WILL MODIFY THESE METHODS SO THEY CORRESPOND TO YOUR GAME"S LOGIC
   void StartLevel1()
    {
        startGameButton.gameObject.SetActive(false);
        Utility.HideCG(cg);
        gameObjectLayers[0].SetActive(true); //level1
        levelText.text = "Level 1";
        StartSpawner(LevelState.level1);
        StartCoroutine(reloadTimer(30));
    }

    //stop level1, prepare for starting level2
    void LoadLevel2()
    {
        player.SetActive(false);
        StopSpawner(LevelState.level1); //- needs changed for l3

        StopAllCoroutines(); //stop timer
        fader.FadeReset(); //fadeOut - fadeIn
        startGameButton.onClick.RemoveAllListeners();
        
        startGameButton.onClick.AddListener(StartLevel2); //- needs changed for l3

        btnText.text = "Start Level 2"; //-- needs changed for l3

        startGameButton.gameObject.SetActive(true);
    }

    //When "Start Level 2" button is selected
    public void StartLevel2()
    {
        gameObjectLayers[0].SetActive(false); //level1 - needs changed for l3
        gameObjectLayers[1].SetActive(true); //level2 - needs changed for l3

        ResetPlayerPosition(); //move player to right edge
        player.SetActive(true);
        StartSpawner(LevelState.level2);// -- needs changed for l3

        levelText.text = "Level 2"; //- needs changed for l3

        startGameButton.gameObject.SetActive(false);
        StartCoroutine(reloadTimer(20));
    }


    void LoadLevel3()
    {
        //add your code here
    }

    public void StartLevel3()
    {
        add your code here
    }


    //What happens when the MiniGame is over due to winning?
    //How does a player leave the MiniGameScene?
   //What happens when the MiniGame is over due to winning?
    //How does a player leave the MiniGameScene: Win or Lose?
    void MiniGameOver()
    {
        if( curLevel == LevelState.win)
        {
            GameData.instanceRef.miniGameWinner = true;
        }
        else
        {
            GameData.instanceRef.miniGameWinner = false;
        }
        if (onMiniGameEnd != null) //some listener (MiniGState)
        {
            onMiniGameEnd.Invoke();
        }
        //invoke custom event to notify MiniGState where sceneChange logic an be executed.
    } //end MiniGameOver



    /// <summary>
    /// Starts the spawner.
    /// </summary>
    /// <param name="level">Level.</param>
    void StartSpawner( LevelState level)
    {
        int index = (int)level - 1; //array index is one less than level number
        Spawner spawner = gameObjectLayers[index].GetComponentInChildren<Spawner>();
        spawner.gameObject.SetActive(true);
        spawner.activeSpawning = true;
        spawner.StartSpawning(); //Make sure to remove this code from Start in the spawner script
    }

    /// <summary>
    /// Stops the spawner.
    /// </summary>
    /// <param name="level">Level.</param>
    void StopSpawner(LevelState level)
    {
        int index = (int)level - 1; //array index is one less than level number
        Spawner spawner = gameObjectLayers[index].GetComponentInChildren<Spawner>();
        spawner.gameObject.SetActive(false);
        spawner.activeSpawning = false;
        spawner.DestroySpawnedObjects(); 
    }


    //moves player to left-edge when new level loaded
    public void ResetPlayerPosition()
    {   
        cameraFollow.ResetPosition();
        player.transform.localPosition = playerSpawnPoint.transform.localPosition;
    }

    /// <summary>
    /// Reloads the Scene when the timer runs out
    /// </summary>
    /// <returns>The timer.</returns>
    /// <param name="reloadTimeInSeconds">Reload time in seconds.</param>
    IEnumerator reloadTimer(float reloadTimeInSeconds)
    {
        float counter = 0;

        while (counter < reloadTimeInSeconds)
        {
            counter += Time.deltaTime;
            timerText.text = "Timer: " + Mathf.RoundToInt(counter).ToString() + " of " + Mathf.RoundToInt(reloadTimeInSeconds).ToString(); ;
            yield return null;
        }
        //Timer is over - if the timer runs out
        ReloadMiniGame();
    }

    /// <summary>
    /// 
    /// </summary>
    public void ReloadMiniGame()
    {
        GameData.instanceRef.ResetMiniGameData();
        Scene curScene = SceneManager.GetActiveScene();
        fader.EndScene(curScene.buildIndex);
        if (StateManager.instanceRef != null)
        {
            StateManager.instanceRef.SwitchState(new MiniGState());  //must re-initialize
        }else{
        Debug.Log("Play Testing MiniGame - no StateManager so can't switch scene");
        }
    }

}// end class
```

