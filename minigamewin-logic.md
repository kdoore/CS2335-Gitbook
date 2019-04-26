#MiniGame Win Lose Logic - Optional 
There are 2 different versions of MiniGState below. Version 1 always loads the EndScene, Version 2 loads the next scene depending on consequences of winning or losing the MiniGame. 

###VERSION 1 - Always Executes: LoadEndScene( )
When the onMiniGame Event occurs, it triggers loading of the EndScene.



```java****

using UnityEngine;
using UnityEngine.UI;
using UnityEngine.SceneManagement;
using UnityEngine.Events;

public class MiniGState : IStateBase {

    Button btnOption1;
    LevelManager levelManager;

    /// <summary>
    /// The scene.
    /// </summary>
    private GameScene scene;

    /// <summary>
    /// Gets the scene number - enum
    /// </summary>
    /// <value>The scene.</value>
    public GameScene Scene
    {
        get { return scene; }
    }

    /// <summary>
    /// Initializes a new instance of the <see cref="T:MiniGState"/> class.
    /// </summary>
    public MiniGState()
    {
        scene = GameScene.MiniGame;
    }


    public void InitializeObjectRefs()
    {
        btnOption1 = GameObject.Find("ButtonOption1").GetComponent<Button>();
        btnOption1.onClick.AddListener(LoadEndScene);

        levelManager = Object.FindObjectOfType<LevelManager>();
        levelManager.onMiniGameEnd.AddListener(MiniGameOver);

    }

    public void MiniGameOver()
    {
        if (GameData.instanceRef.miniGameWinner)
        {
            LoadWinScene(); //we won
        }
        else
        {
            LoadLoseScene();
        }

    }

    public void LoadEndScene()
    {
        SceneManager.LoadScene("EndScene");
        StateManager.instanceRef.SwitchState(new EndState());
    }
}

```


###VERSION 2 - Conditional Logic Determines next Scene to Load
The code below shows how you can use the LevelManager custom UnityEvent: onMiniGameEnd.  In the example code below, when the event occurs, then the MiniGameOver method is executed, it checks GameData, to determine if the player won or lost the MiniGame, and it determines the next scene based on that logic.


```java

using UnityEngine;
using UnityEngine.UI;
using UnityEngine.SceneManagement;
using UnityEngine.Events;

public class MiniGState : IStateBase {

    Button btnOption1;
    LevelManager levelManager;

    /// <summary>
    /// The scene.
    /// </summary>
    private GameScene scene;

    /// <summary>
    /// Gets the scene number - enum
    /// </summary>
    /// <value>The scene.</value>
    public GameScene Scene
    {
        get { return scene; }
    }

    /// <summary>
    /// Initializes a new instance of the <see cref="T:MiniGState"/> class.
    /// </summary>
    public MiniGState()
    {
        scene = GameScene.MiniGame;
    }


    public void InitializeObjectRefs()
    {
        btnOption1 = GameObject.Find("ButtonOption1").GetComponent<Button>();
        btnOption1.onClick.AddListener(LoadEndScene);

        //Connect with LevelManager Event: onMiniGameOver
        levelManager = Object.FindObjectOfType<LevelManager>();
        levelManager.onMiniGameEnd.AddListener(MiniGameOver);

    }

    public void MiniGameOver()
    {
        if (GameData.instanceRef.miniGameWinner) //check GameData
        {
            //LoadWinScene(); //we won
        }
        else
        {
            //LoadLoseScene();
        }

    }

    public void LoadEndScene()
    {
        SceneManager.LoadScene("EndScene");
        StateManager.instanceRef.SwitchState(new EndState());
    }
}


```

