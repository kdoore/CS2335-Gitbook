#MiniGameManager_v2


```java
using UnityEngine;
using UnityEngine.UI;

public enum MiniGame_v2State { idle, active, win, lose }


public class MiniGameManager_v2 : MonoBehaviour
{

    public MiniGame_v2State curGameState = MiniGame_v2State.idle;
    public Button startButton; //set in inspector
    public int winScore = 30;
    public GameObject pickUp_Parent;

    // Use this for initialization
    void Start()
    {
        startButton.onClick.AddListener(StartGame);
        StartGame(); //calls method with StartGame Logic
        //StartGame() disables startButton, so enable it at start
        startButton.gameObject.SetActive(true); //displays the StartButton 
    }

    // Update is called once per frame
    void Update()
    {
        if (curGameState == MiniGame_v2State.active)
        {
            if (GameData.instanceRef.Health > 0) //still have health
            {
                if( GameData.instanceRef.Score >= winScore)
                {
                    curGameState = MiniGame_v2State.win;
                  
                    GameOver();
                }
            }
            else //we have no health
            {
                curGameState = MiniGame_v2State.lose;
              
                GameOver();
            }
        }
    } //end update

    private void GameOver()
    {
        startButton.gameObject.SetActive(true); //set gameObject with Button Component to active
    }


    /// <summary>
    /// starts game.
    /// Executed by Unity when the StartButton is clicked
    /// </summary>
    public void StartGame()
    {
        GameData.instanceRef.ResetGameData();
        pickUp_Parent.SetActive(true);
        curGameState = MiniGame_v2State.active; //FSM - change state
        startButton.gameObject.SetActive(false); //disables the StartButton GO
    }
}

```

