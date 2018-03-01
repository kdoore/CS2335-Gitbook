#Simple Dialog Prefab

How can we create a very simple dialog system in Unity?  
We should try to make something that will work as a prefab, so we can use it in any scene, as many times as we'd like. 
- We need to include
    - UI-panel as a container with a CanvasGroup component   so we can toggle it's visibiltiy
    - UI-Text, a child of the UI-panel, this will display the text
    - UI-Button, a child of the UI-panel, this will allow us to advance through the dialog items
    - A custom script component- attached to the UI-Panel
    will have logic for the dialog system.
      -Script must use a public List of strings that can be populated in the inspector, with new dialog each time the panel-prefab is used.  

 ###Prefab GameObject: Hierarchy     
 The image below shows the Hierarchy panel structure of these gameObjects.  The DialogController.cs script is attached to the top-level panel object.  The panel must also have a CanvasGroup component attached. In the Inspector panel: Add Component>Layout > CanvasGroup
 
![](/assets/Screen Shot 2018-03-01 at 4.06.32 PM.png)

The DialogText is anchored to 4 corners of it's parent, the SimpleDialogPanel.  The NextButton is anchored to the bottom -right corner of the panel.

![](/assets/Screen Shot 2018-03-01 at 4.26.54 PM.png)


