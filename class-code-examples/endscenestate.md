#EndState

Your game details will determine the content of this script file.  You will want to add some UI-Text elements that can have the content modified based on the results of the MiniGame, and other data stored in GameData.  Below is an example.
Checks **GameData: miniGameWinner** to determine if player won the miniGame.

This code assumes you have 2 UI-Text elements
- ResultsText
- InventoryCountText

###Updated EndState code with Consequences from MiniGame


```java

using UnityEngine;
using UnityEngine.UI;
using UnityEngine.SceneManagement; //add to all State Files

public class EndState : IStateBase
{
    private Text resultsText;
    private Text inventoryCountText;
    private int itemCount = 0;

    private GameScene scene;
   
    //add commenets
    public GameScene Scene
    {
        get { return scene; }
    }

    //GameScene objectRefs
    private Button startBtn;

    //constructor  // add comments
    public EndState()
    {
        scene = GameScene.End;
    }

    //add comments
    public void InitializeObjectRefs()
    {
        resultsText = GameObject.Find("ResultsText").GetComponent<Text>();
        inventoryCountText = GameObject.Find("InventoryCountText").GetComponent<Text>();
        if (CheckGameStats()== true)
        {
            resultsText.text = "You are a winner";
            inventoryCountText.text = "You have: " + itemCount + " items";
        }
        else
        {
            resultsText.text = "Sorry that you didn't win the game";
            inventoryCountText.text = "You have: " + itemCount + " items";
        }
        startBtn = GameObject.Find("StartButton").GetComponent<Button>();
        startBtn.onClick.AddListener(LoadBeginScene);
        Debug.Log("Add Debug Info");
    }

    bool CheckGameStats()
    {
        Inventory inventory = GameData.instanceRef.inventory;
        if( inventory.inventory.Count > 0)
        {
            Debug.Log("Inventory Has Items");
            itemCount = inventory.inventory.Count;
        }
        return GameData.instanceRef.miniGameWinner;
    }

    public void LoadBeginScene()
    {
        Debug.Log("Add Debug Info");
        SceneManager.LoadScene("BeginScene");
        StateManager.instanceRef.SwitchState(new BeginState());

    }
} //end class:  EndState
```

