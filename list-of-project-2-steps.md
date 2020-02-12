#List of Steps for Project 2

Below is an listing of steps for modifying  the TextAdventure starter-code, for completing Project2.  

Gitbook page:  [Create new scene, state](/project-2-create-new-scene-and-state.md)

1. Create 3 new scenes

2.  For each scene:  add scene to the Unity Build Settings 

	Add UI Canvas in Scene
	Configure:  Screen-Space Camera
	Set camera to: MainCamera
	Add label to identify scene
	Add 2 Buttons, to jump to other scenes

3.  Add scene to StateManager.cs GameScene enums

4.  Create a new SomeStates.cs classes, add code so it is similar to BeginState.cs

5.  Modify code in SomeScene.cs - 

	- Add code for IStateBase interface:
		- GameScene Scene
		- InitializeObjectRefs( ){  }
			
	- Constructor:  
		- public SomeScene( )  
		- set: scene = GameScene.someScene;
		 		
6.  Modify BeginScene - Add button to go to SomeScene

7.  Modify BeginState - Add code for button to go to SomeScene
		
	- Define object-reference variable
	-  Initialize variable in:  InitializeObjectRefs( );
	-  Create custom method:  public void LoadSomeScene( )
	-  Configure using similar code in from BeginState.cs for LoadEndScene( )

	- 2 lines of code for to switching scene / state:
	
	 `SceneManager.LoadScene ("EndScene");  //actual scene name 	`	`StateManager.instanceRef.SwitchState (new EndState ());  `

###Customize each Scene

8.  Add 2 buttons to all scenes, code for button event logic in each state.cs file

9.  Add a panel that has a CanvasGroup component, where a button on the panel can hide the panel 

10.  Add background images (UI Image) to each scene

11.  Add some text / dialog and a label to each scene

12.  Add List of links for credits for assets used in the project in the final scene.


