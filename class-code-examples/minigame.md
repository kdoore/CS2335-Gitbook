#MiniGState

Code has been updated to include logic for listening for Event: 
**LevelManager event: onMiniGameEnd**
 
 In this simple case, the event causes a scene-change to the EndScene.  Logic in the EndState.cs script displays different content depending on whether GameData shows that Health is not less than or equal to 0.



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

        levelManager = Object.FindObjectOfType<LevelManager>();
        levelManager.onMiniGameEnd.AddListener(LoadEndScene);
    }

    public void LoadEndScene()
    {
        SceneManager.LoadScene("EndScene");
        StateManager.instanceRef.SwitchState(new EndState());
    }
} //end class

```

