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
   1.  Add the dialogPanel prefab to the scene.
       **Hierarchy:  DialogPanel Details**
       - DialogPanel  
       - with CanvasGroup
       - remove Image(Script)
       - Add DialogManager script component
       - Child GameObjects (Use thie ordering)
           - DialogText
           - NextDialogButton
           - CharacterImage

                      
   2.  Add an OpenDialogButton to the scene.
   
   
   
   
        
        
        
    