# Project 2 - List of Steps

Below is an listing of steps for modifying the for completing Project2.

The link below gives more detailed information for Gitbook: [Create new scene, state](/project-2-create-new-scene-and-state.md)

1. Create a total of 5 scenes (MiniGame counts as one of your scenes)

2. For each scene: add scene to the Unity Build Settings

3. Add UI Canvas in Scene

   * Configure: Screen-Space Camera
   * Set camera to: MainCamera

4. Add label to identify scene

5. Add 2 Buttons, to jump to other scenes

6. Add scene to StateManager.cs GameScene enums

7. Create a new SomeStates.cs classes, add code so it is similar to BeginState.cs

   * Modify code in SomeScene.cs
   * Add code for IStateBase interface:
   * GameScene Scene
   * InitializeObjectRefs\( \){ }
   * Constructor:
     * public SomeScene\( \)
     * set: scene = GameScene.someScene;

8. Modify BeginState - Add code for button to go to SomeScene

   * Define object-reference variable
   * Initialize variable in: InitializeObjectRefs\( \);
   * Create custom method: public void LoadSomeScene
   * Configure using similar code in from BeginState.cs for LoadEndScene\( \)
   * 2 lines of code for to switching scene/state 
     * `SceneManager.LoadScene ("EndScene"); //actual scene name`
     * `StateManager.instanceRef.SwitchState (new EndState ());` 


##Troubleshooting Issues when trying to change scenes

**1. StartGame in BeginScene**
The most important thing to check is that you're always starting your game in the BeginScene.  Otherwise, the StateManager will have incorrect code executing, because we hard-coded the starting state in StateManager to be the BeginState.  

If it’s a button to jump scenes that’s not working, then there are several things to check.  

**Check the console** when you run the project, if you have any errors, that's good, that is where to start.  

There are 2 types of errors you may get.  

**Console Message:** **BIG PROBLEMS** - scene state mismatch.  

These are the things to check:

   - **Unity Build Settings** Open the [Unity build settings](https://kdoore.gitbooks.io/cs-2335/content/project-2-create-new-scene-and-state.html#important-add-scenes-in-unity-build-settings) (file >Build Settings), look at the scene ID’s, compare with the GameScene enums in StateManager, these must match perfectly.
   
  ```java
 public enum GameScene
{
    Begin = 0,
    Scene2 = 1,
    MiniGame =2,
    Scene3 = 3,
    End = 4
}
```
   
   - ** SceneXState.cs Constructor** within a SceneXState.cs file.  In the constructor, you need to set the scene correctly, example:
   
```java
public SceneXState(){
     scene = GameScene.SceneX; //Scene-State Mismatch Error if incorrect
     }
```

    - ** Button Logic** in the SceneXState file that you are trying to leave from - prior to the error:  BIG PROBLEMS comment.
 
Make sure that: logic is correct for the button event: the scene / state must match.
 the LoadScene( "SceneName" )
matches the SwitchState( new StateName(  ));

```java
  public void LoadScene2()
    {
        Debug.Log("Leaving BeginState going to Scene2State");
        SceneManager.LoadScene("Scene2"); //UNITY METHOD TO CHANGE SCENE //actual scene name
        StateManager.instanceRef.SwitchState(new Scene2State());  //create new state, pass to StateManager
    }
```

   - **Button Initialization Logic Error: **  
In the code below, there's a mistake because all logic is being added to the btnOption1.onClick method.  Look for these types of errors.

```java

public void InitializeObjectRefs()
    {
        btnOption1 = GameObject.Find("ButtonOption2").GetComponent<Button>();
        btnOption1.onClick.AddListener(LoadEndScene);

        btnOption2 = GameObject.Find("ButtonOption1").GetComponent<Button>();
        btnOption1.onClick.AddListener(LoadScene2); //ERROR HERE

        Debug.Log("BeginState InitializeObjRefs");
    }
```

