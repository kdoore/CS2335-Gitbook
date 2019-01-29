#PlayerStatsDisplay - Version 1

Player Stats Display will be our first use of UI-GameObjects.  

We'll Use one UI-Panel and 2 UI-Text elements to display the Score and Health values that are stored in the GameData  component.

Steps Create GameObjects: 
- UI-Panel:  ScorePanel
- UI-Text:  ScoreText
- UI-Text:  HealthText

![](/assets/Screen Shot 2019-01-29 at 6.23.51 AM.png)

See the following Pages for more info on working with UI Elements: 
[UI Elements: link](/project-1-score-and-ui-elements.md)

**Details:**
1.  Add 1 UI-Panel Element to the Hierarchy, name it:  **ScorePanel**
2.  This initial creation of a UI element actually causes creation of 3 gameObjects that are added to the Hierarchy:
    - Canvas - the parent GameObject for all UI elements
    - Panel - used here as a container for 2 UI-Text elements
    - EventSystem - manages UI event-handling - if UI events don't seem to be working, make sure that one of these components is in the Hierarchy.  
    - When any additional UI elements are added to the Hierarchy, the Canvas will be the parent element.
    - Modify the Canvas's RenderMode, set it to 'Screen-space Camera', and select the Main-camera as the render camera
    - Resize the Panel and Use the Rect-Transform component to set the Panel's anchors to the top of the canvas.
  - Add UI-Text element: name it: ** ScoreText **
  - Use Rect-Transform Component panel to set the  anchors of the ScoreText so it is aligned to the Left Side of the Panel.  
  - Add UI-Text element: name it: ** HealthText **
  - Use Rect-Transform Component panel to set the  anchors of the ScoreText so it is aligned to the Right Side of the Panel.  

 
Attach the PlayerStats script to the ScorePanel gameObject.  Populate the Inspector fields: ScoreText and HealthText in the Inspector using the ScoreText and HealthText GameObjects.

The image below shows the ScorePanel has 2 UI-Text gameObjects as children in the Hierarchy Panel.  In the Inspector Panel, the ScoreText has it's Rect Transform set to Upper-Left. The ScoreText's Transform is defined relative to it's Parent - the ScorePanel
![](/assets/Screen Shot 2019-01-29 at 6.26.07 AM.png)

The image below shows that the ScorePanel is anchored using stretch-top setting in the Rect Transform component.  Also, the HealthText and ScoreText gameObjects have been used to populate the corresponding fields in the PlayerStats script component.
![](/assets/Screen Shot 2019-01-29 at 5.57.40 AM.png)
                      
###Class: PlayerStats.cs:

```java

using System.Collections;
using System.Collections.Generic;
using UnityEngine;
using UnityEngine.UI;

public class PlayerStats : MonoBehaviour {

    //make connection with Text elements in the inspector
    public  Text healthText, scoreText;
    // Use this for initialization

    void Start()
    {
        UpdateDisplay();
    }

    void Update(){ //called every frame - polling to see if data changed
        UpdateDisplay();
    }

    public void UpdateDisplay(){
        healthText.text = "Health: " + GameData.instanceRef.Health;
        scoreText.text = "Score: " + GameData.instanceRef.Score;

    }
} // end Class
 
```


    