#EndState

Your game details will determine the content of this script file.  You will want to add some UI-Text elements that can have the content modified based on the results of the MiniGame, and other data stored in GameData.  Below is an example.


This code assumes you have 2 UI-Text elements
- ResultsText
- InventoryCountText

###Updated EndState code with Consequences from MiniGame


```java

using UnityEngine;
using UnityEngine.UI;
using System.Collections;
using UnityEngine.SceneManagement; //add to all State Files

public class EndState : IStateBase
{
    private bool winner = false;
    private Text resultsText;
    private Text inventoryCountText;
    private int count = 0;

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
        if (winner)
        {
            resultsText.text = "You are a winner";
            inventoryCountText.text = "You have: " + count + " items";
        }
        else
        {
            resultsText.text = "Sorry that you didn't win the mini-game";
            inventoryCountText.text = "You have: " + count + " items";
        }
        startBtn = GameObject.Find("StartButton").GetComponent<Button>();
        startBtn.onClick.AddListener(LoadBeginScene);
        Debug.Log("Add Debug Info");
    }

    bool checkGameStats()
    {
        Inventory inventory = GameData.instanceRef.inventory;
        if( inventory.inventory.Count > 0)
        {
            Debug.Log("Inventory Has Items");
            count = inventory.inventory.Count;
        }
        if( GameData.instanceRef.Health > 0)
        {
            winner = true;
        }
        return winner;
    }

    public void LoadBeginScene()
    {
        Debug.Log("Add Debug Info");
        SceneManager.LoadScene("BeginScene");
        StateManager.instanceRef.SwitchState(new BeginState());

    }
} //end class:  EndState

```

