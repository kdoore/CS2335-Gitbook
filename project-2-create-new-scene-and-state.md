#Project 2 - Create New Scene and State

This section will provide details for how to create a Scene and corresponding state, where these items will be added to a copy of Project1, **rename to Project2**.  This assumes you have either downloaded the Proj2 starter-code unity-package, or the Proj2 starter-code assets folder. See: [Starter Assets](/project2-starter-assets.md)

###Create New Scene, Add Scene to Build Settings
To create a new scene, right-click in the Unity Project / Assets Panel and select the Create > Scene option.  Name this scene using a descriptive name according to your project's theme.  Next, Add this scene to your project's build settings, following the instructions in the main Project2 page: 

##IMPORTANT: Add Scenes in Build Settings
For this project to work, you must first go into your project's Build Settings. (File > Build Settings)  You must add all scenes to the Scenes to Build Panel.  You can drag the scenes from your assets directly into the Scenes to Build Panel.  **Note the order, and SceneID's on the right side, this must match GameScene Enums in StateManager**, BeginScene must be higher than EndScene in the order, it will have a 0 on the right side of the panel, while EndScene will have a 1.

![](/assets/Unity 2.JPG)

![](/assets/Screen Shot 2018-02-12 at 2.40.58 PM.png)




###Add GameScene Enum to StateManager.cs script:


```java
/// <summary>
/// Game Scene. Matches Unity Scenes in Build Settings
/// </summary>
public enum GameScene
{
	Begin = 0,
	End = 1, 
	Scene2 = 2  //Create an Enum value for each scene in your Unity Build Settings - Scenes In Build

}
```
###Configure the Canvas to use Screen-Space Camera
[ See: Configure Canvas to use Screen-Space Camera](https://kdoore.gitbooks.io/cs-2335/content/project-1-score-and-ui-elements.html#animation-set-canvas-render-mode-to-screen-space-camera) 

###Create a StateX Script file to correspond to the new Scene
Create a new C# Script in the scripts folder of your project's asset panel.  Name the script so it's obvious this script corresponds to the scene you just created.  Then, we'll need to modify the code for this script so that it is similar to the other StateX files that were included in the starter code, such as BeginState.

###Change Script Steps:
 - Add 'using' directives for SceneManagement and UI to the top of the script file:


```java
using UnityEngine.UI;
using UnityEngine.SceneManagement;

```

- Remove :MonoBehaviour ( Base-Class )
- Add: :IStateBase ( interface )
- Remove:  Unity Start( ) and Update( ) event functions
- Add code for IStateBase, Property: Scene  
          

```java

public class SomeState : IStateBase
{
    /// <summary>
	/// The scene enum associated with this state.
	/// </summary>
	private GameScene scene;

	/// <summary>
	/// Read Only Property gives access to the scene number - enum
	/// </summary>
	/// <value>The scene.</value>
	public GameScene Scene {
		get{ return scene; }
	}

//additional code must be added, see below	
} //end class
    
```
- Add code for IStateBase, Method:  InitializeObjectRefs()


```java

	/// <summary>
	/// Similar to Unity Start() 
	/// exectued once, after scene is loaded - called from StateManager
	/// Used to initialize object references - can be used to cache object references
	/// </summary>
	public void InitializeObjectRefs ()
	{
	  //code will be added for initializing object refs for scene buttons
	  Debug.Log("In InitializeObjectRefs for SomeState ");
	}
```

 - Add Class Constructor, and set the corresponding GameScene Enum.
  
     

```java
 public SomeState ()
	{
		scene = GameScene.SomeScene;
		Debug.Log("In Constructor for SomeState");
	}

```

###Add A Button to the BeginScene to the new Scene.
Now you must add buttons to other scenes, and code to other state scripts so you can get to this new scene.

To add a Button to the BeginScene, select: GameObject > UI > Button.  Name this button so that it corresponds to the task it will be used for.  For example, we can call it: SomeSceneButton

###Add Code to BeginState for the new Button's Function

- Declare Object-Reference Variable for a Button Component


```java

private Button someSceneBtn;
```
- Initialize the object reference in: InitializeObjectRefs( )


```java

public void InitializeObjectRefs ()
	{
		someSceneBtn = GameObject.Find ("SomeSceneButton").GetComponent<Button> ();
		someSceneBtn.onClick.AddListener (LoadSomeScene); //call custom method defined below
		Debug.Log ("In BeginState initializeObjRefs");
	}

```
- Write Custom Method to change scene and state


```java

/// <summary>
	/// Event handler - called when endBtn is clicked
	/// Loads the end scene.
	/// </summary>
	public void LoadSomeScene ()
	{  
		Debug.Log ("Leaving BeginScene going to SomeScene");
		SceneManager.LoadScene ("SomeScene");  //actual scene name
		StateManager.instanceRef.SwitchState (new SomeState ());  //create new state, pass to StateManager
	}
```

- Use the Button to go to the new Scene, Add Images,2 Buttons to each of 5 scenes  

