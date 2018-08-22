#UI Elements 

User Interaction Elements provide easy to configure gameObjects that allow for user interaction with the game.   

In this section, we'll discuss how to create and position UI elements in a scene.

###Canvas
All UI elements are children of the Canvas class.  When the first UI element is added to a scene, in addition, a Canvas gameObject and an EventSystem gameObject are also added to a scene, these are required to be in the Hierarchy for every scene that has UI elements to be displayed and have interactivity.  

###Canvas: ScreenSpace-Overlay
When the Canvas is added to a scene, by default, it's layout is offset from the main camera.  For our purposes, it is easiest to configure the Canvas Component so that it's rendered in the same position as the regular game scene.  To do that, we need to change the Render-Mode of the Canvas, we'll change it from Screen-Space Overlay to `Screen Space - Camera`, and we'll select the `Main Camera` as the `Render Camera` for the Canvas Component.  See image and animation below to configure the `Render Mode` of the Canvas to be `Screen Space Camera`, with the `Render Camera` set to `Main Camera`.   

![](Screenshot 2016-02-15 20.14.52.png)


###UI-Button, Child-GameObject: Text


Add a UI Button to the game scene.  Name the button 'StartButton'.  The StartButton has a child gameObject: Text. Configure the Text GameObject, so it's Text Component', Text field has a value of 'Start Game'.  See the animation below to add a StartButton gameObject to the scene.

###Set Position, Color of Button, 
Use the RectTransform tool to set the anchors and position of the button to the center of the scene.  Set the 'normal' and 'highlight' colors of the button, so that you'll be able to see the button's highlight state when moused-over.

 ###Animation: Set Canvas Render Mode to Screen Space-Camera
The animation below shows that when you add a UI element, (such as a Button GameObject, to your scene, a Canvas and EventSystem are also added to the Hierarchy.  Then you need to configure the Canvas GameObject so it's set to Screen Space-Camera as described above.

![](http://g.recordit.co/YVfLJCsBTe.gif)

###UI-Panel and Text for Score
Add a UI-Panel to the scene, name it: 'ScorePanel'.  Add a Text gameObject as a **child** of the 'ScorePanel', name this 'ScoreText'.  For the 'ScorePanel', set the anchors to that it's anchored to the upper right corner of the scene.  Configure the 'ScoreText', text element, so it is stretched to fit the 'ScorePanel', and set the starting text value to show 'Score: 0', with BestFit and alignment center, center as shown in the image below. Select a color for the panel and text so they will be visible during gameplay.

![](/assets/Screen Shot 2018-01-20 at 6.41.40 PM.png)

###Animation:  Add UI Panel and UI Text.
The animation below shows how to add a UI-Panel, then how to add a UI-Text element and make that a child of the UI-Panel.  Next, the panel is resized using the rect tool in the upper-right tools menu.The panel's Rect Transform is set so that the panel is anchored to the upper-right corner of the scene, we can see 4 triangles clustered in the upper-right corner showing that the all 4 anchors are set to the upper-right corner of the canvas.  Then the Text has it's Rect Transform set so it's stretched and anchored to it's parent, the UI Panel.  Then Best Fit is selected for the text, and it's color is changed to white, it's Text field is set to display: Score: 0.  The UI-Panel should be named: ScorePanel, the UI-Text should be named: ScoreText

![](http://g.recordit.co/EQdaJ1Vbrx.gif)

###UI-Panel and Text for GameOver Notification.
Create a UI-Panel and a child-gameObject that is a Text element, that'll be used for the display of the GameOver notification.  Name the Panel-gameObject:  'GameOverPanel' and name the Text-gameObject: 'GameOverText'.  Configure the rect-transform for the Text so it's stretched to fit the Panel, anchor it to the corners of it's parent. Resize the Panel and move to the center of the scene.  Set the color of the panel and text so they are visible during gameplay.  

![](/assets/Screen Shot 2018-01-20 at 6.51.12 PM.png)


