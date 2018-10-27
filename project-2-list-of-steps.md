#Project 2 - List of Steps

Below is an listing of steps for modifying the TextAdventure starter-code, for completing Project2. 

The link below gives more detailed information for Gitbook: [Create new scene, state](/project-2-create-new-scene-and-state.md)

    1. Create a total of 5 scenes (completed in project 1)
    
    2. For each scene: add scene to the Unity Build Settings
    
    3. Add UI Canvas in Scene
        - Configure: Screen-Space Camera
        - Set camera to: MainCamera
        
    4. Add label to identify scene
    5. Add 2 Buttons, to jump to other scenes
    
    6. Add scene to StateManager.cs GameScene enums
    
    - Create a new SomeStates.cs classes, add code so it is similar to BeginState.cs
    
        - Modify code in SomeScene.cs
        - Add code for IStateBase interface:
        - GameScene Scene
        - InitializeObjectRefs( ){ }
        - Constructor:
            - public SomeScene( )
            - set: scene = GameScene.someScene;
            
    7. Modify BeginState - Add code for button to go to SomeScene
        - Define object-reference variable
        - Initialize variable in: InitializeObjectRefs( );
        - Create custom method: public void LoadSomeScene
        - Configure using similar code in from BeginState.cs for LoadEndScene( )
        - 2 lines of code for to switching scene/state 
           - `SceneManager.LoadScene ("EndScene"); //actual scene name`
           - `StateManager.instanceRef.SwitchState (new EndState ());` 
    
###Tips for Adding Dialog to 3 Scenes     
           
   Adding UI elements to each scene for narrative or dialog. 
   See the updated [DialogManager](/conversation-scriptable-objects/dialogmanagerconvlist.md) script, unityPackage, unityProject with prefabs for DialogManager, ConversationList scriptableObject. 
   
   For each of 3 scenes.
   1.  Add the **DialogPanel prefab** to the scene.
   
       Hierarchy:  DialogPanel Prefab Details
       - DialogPanel  - UI-Panel
       - add CanvasGroup
       - remove Image(Script)
       - Add DialogManager script component
       - Child GameObjects (as ordered below)
           - CharacterImage 
           - NameText
           - TextPanel
               - DialogText
               - NextDialogBtn
          
       - Configure DialogManager Script component - see details below
            
![](/assets/Screen Shot 2018-10-27 at 9.47.57 AM.png)

     2.  Add an OpenDialogButton to the scene.
     3.  Add a DecisionPanel to the scene.
         - Panel with CanvasGroup
         - Add Buttons  to change scenes or to open a new DialogPanel.
             - Option1Btn
             - Option2Btn
     4. Create a new ConversationList ScriptableObject for each dialog.  
     - You can create a duplicate of an existing one, or you can use the asset panel's right-click menu to create ScriptableObject.
     - Create ConversationList Elements, fill with DialogText, CharacterImage, CharacterName
         
    5. Configure DialogManager Script Component in Inspector Panel
    
    **Inspector Panel: DialogPanel Details**
    - Configure DialogManager Script component
        - Select Conv List: ConversationList - ScriptableObject
        - Select Next Panel To Open - with DecisionPanel - gameObject
        - Select Open Dialog Btn - with OpenDialogBtn - gameObject
        - Select Show On Start if not using OpenDialogBtn.
        
    Image shows how to configure DialogManager Script Component
    
![](/assets/Screen Shot 2018-10-27 at 6.26.21 AM.png)

         
     - **Configure logic in SceneXState.cs file for buttons to do scene transition:**
         - Find Button by name in `InitializeObjectRefs( )` 
         - Write `LoadSceneX` methods
         - Add LoadSceneX method as listeners to Button.onClick event handler.
         
```java
       public void InitializeObjectRefs ()
	{
	    option1Btn = GameObject.Find ("Option1Btn").GetComponent<Button> ();
		option1Btn.onClick.AddListener (LoadScene2);

        option2Btn = GameObject.Find("Option2Btn").GetComponent<Button>();
        option2Btn.onClick.AddListener(LoadScene3);
        Debug.Log ("Inititalize Object Refs for Scene1State");
	}
       
 public void LoadScene2()
    {
        Debug.Log("Leaving Scene1, going to Scene2");
        SceneManager.LoadScene("Scene2");  //actual scene name
        StateManager.instanceRef.SwitchState(new Scene2State());  //create new state, pass to StateManager

 public void LoadScene3()
    {
        Debug.Log("Leaving Scene1, going to Scene3");
        SceneManager.LoadScene("Scene3");  //actual scene name
        StateManager.instanceRef.SwitchState(new Scene3State());  //create new state, pass to StateManager
    }
  
```


         
   
   
   
   


        
        
    