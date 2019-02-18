#Project 2 Code

##Starter Code: 

###StateManager.cs


```java

using UnityEngine;
using UnityEngine.UI;
using System.Collections;
using UnityEngine.SceneManagement;

///
/// CS2335 - Spring 19
///  
/// <summary>
/// Game Scene. Matches Unity Scenes in Build Settings
/// </summary>
public enum GameScene
{
    Begin = 0,
    End = 1
}

/// <summary>
/// State manager.  Add comments
/// </summary>
public class StateManager : MonoBehaviour
{
    public static StateManager instanceRef;
    public IStateBase activeState;
    public GameScene curScene;

    /// <summary>
    /// Create this singleton instance.
    /// Implement the Singleton Design Pattern - initialize managerInstance
    /// </summary>
    void Awake()
    {
        if (instanceRef == null)
        {
            instanceRef = this;
            DontDestroyOnLoad(gameObject);  //the gameObject this is attached to 
        }
        else
        {   //
            DestroyImmediate(gameObject);
            Debug.Log("Destroy GameObject");
        }

    }
    // Use this for initialization
    void Start()
    {
        activeState = new BeginState(); //MUST MATCH YOUR CUSTOM CODE
        curScene = activeState.Scene;
        activeState.InitializeObjectRefs();  //call to Initialize BeginState object references

        //next scene change - this event will call OnLevelFinishedLoading custom function
        SceneManager.sceneLoaded += OnLevelFinishedLoading;  //add function to sceneLoaded delegate

    }

    /// <summary>
    /// Switchs the state.
    /// </summary>
    /// <param name="newState">New state.</param>
    public void SwitchState(IStateBase newState)
    {
        activeState = newState;
        curScene = newState.Scene;
        Debug.Log("Add Debug Info");
    }

    //StateManager needs to know when a new scene has been loaded
    //then it can call: InitializeObjectRefs for the current state
    void OnLevelFinishedLoading(Scene scene, LoadSceneMode mode)
    {
        int sceneID = scene.buildIndex;
        if (sceneID == (int)curScene)
        {
            Debug.Log("New function in StateManager to initialize new scene objects - exectued");
            activeState.InitializeObjectRefs();
        }
        else
        {
            Debug.Log("Big Problems - Scene & State Mismatch");
            Debug.Log("LevelFinished Loading :Scene Loaded: " + sceneID + " ActiveState Scene Enum: " + activeState.Scene);
        }


    }
}

```

##IStateBase.cs


```java

using UnityEngine;
using System.Collections;

///
/// CS2335 - Spring 19
///  
/// <summary>
/// I state base.  
/// Interface for all StateX.cs classes  
/// </summary>
public interface IStateBase
{
    /// <summary>
    /// Gets the scene number - enum
    /// </summary>
    /// <value>The scene.</value>
    GameScene Scene
    { // Inteface Property 
        get; //read-only
    }

    //all interface methods are public by default!
    ///<summary>
    /// Similar to Unity Start()  
    /// exectued once, after scene is loaded - called from StateManager
    /// Used to initialize object references - can be used to cache object references
    /// </summary>
    void InitializeObjectRefs();

} //end Interface: IStateBase

```


##BeginState.cs


```java
using UnityEngine;
using UnityEngine.UI;
using System.Collections;
using UnityEngine.SceneManagement; //add to all State Files

public class BeginState : IStateBase
{

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

    //GameScene objectRefs
    private Button endBtn;

    /// <summary>
    /// Initializes a new instance of the <see cref="BeginState"/> class.
    /// </summary>
    /// <param name="manager">Manager.</param>
    public BeginState()
    {
        scene = GameScene.Begin;
    }

    /// <summary>
    /// Similar to Unity Start() 
    /// exectued once, after scene is loaded - called from StateManager
    /// Used to initialize object references - can be used to cache object references
    /// </summary>
    public void InitializeObjectRefs()
    {
        endBtn = GameObject.Find("EndButton").GetComponent<Button>();
        endBtn.onClick.AddListener(LoadEndScene);
        Debug.Log("Add Debug Info");
    }

    /// <summary>
    /// Event handler - called when endBtn is clicked
    /// Loads the end scene.
    /// </summary>
    public void LoadEndScene()
    {
        Debug.Log("Add Debug Info");
        SceneManager.LoadScene("EndScene");  //actual scene name
        StateManager.instanceRef.SwitchState(new EndState());  //create new state, pass to StateManager
    }
} //end class BeginState

```

## EndState.cs
Example code for a Scene_X_State class

```java

using UnityEngine;
using UnityEngine.UI;
using System.Collections;
using UnityEngine.SceneManagement; //add to all State Files

public class EndState : IStateBase
{

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
    public void StateUpdate()
    {

    }

    //add comments
    public void InitializeObjectRefs()
    {
        startBtn = GameObject.Find("StartButton").GetComponent<Button>();
        startBtn.onClick.AddListener(LoadBeginScene);
        Debug.Log("Add Debug Info");
    }


    public void LoadBeginScene()
    {
        Debug.Log("Add Debug Info");
        SceneManager.LoadScene("BeginScene");
        StateManager.instanceRef.SwitchState(new BeginState());

    }
} //end class:  EndState

```

