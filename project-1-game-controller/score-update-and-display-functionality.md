#Score: Update and Display Functionality

This section will focus on all of the GameObjects and scripts that work together to display the updated score.

**Scripts Involved:**
1.  Basket.cs
2.  Apple.cs
3.  Rock.cs
4.  GameController.cs

**GameObjects Involved:**
1. Apple prefab
2. Rock prefab
3. Basket
4. ScorePanel
5. ScoreText
6. StartButton

###Steps to Create UI-Panel, UI-Text to Display Score
1. Add a UI-Panel gameObject to the scene, name it: 'ScorePanel'.  
2. Add a UI-Text gameObject as a **child** of the 'ScorePanel', name this 'ScoreText'.  
3. For the 'ScorePanel',use the RectTransform Anchor tool to set the anchors to that it's anchored to the upper right corner of the scene.  (All anchor triangles are at the upper right corner of the scene - watch animation)
4. Configure the 'ScoreText', text element, so it is stretched and anchored to fit the 'ScorePanel'. (anchor triangles are at each corners of the text and panel - see image below)
5. Set the starting text value to show 'Score: 0', with **BestFit** selected. 
6. Set paragraph alignment: center-horizontal, center-vertical, if desired
7. Select a color for the panel and text so they will be visible during gameplay.

![](/assets/Screen Shot 2018-01-20 at 6.41.40 PM.png)

###Animation: Create & Configure UI Panel and UI Text.
The animation below shows how to add a UI-Panel, then how to add a UI-Text element and make that a child of the UI-Panel.  Next, the panel is resized using the rect tool in the upper-right tools menu.The panel's Rect Transform is set so that the panel is anchored to the upper-right corner of the scene, we can see 4 triangles clustered in the upper-right corner showing that the all 4 anchors are set to the upper-right corner of the canvas.  Then the Text has it's Rect Transform set so it's stretched and anchored to it's parent, the UI Panel.  Then Best Fit is selected for the text, and it's color is changed to white, it's Text field is set to display: Score: 0.  The UI-Panel should be named: ScorePanel, the UI-Text should be named: ScoreText

![](http://g.recordit.co/EQdaJ1Vbrx.gif)

###Code Changes associated with Updating and Displaying Score
The first code to modify is the code in the GameController.cs script by creating a method: UpdateScore(), that updates the score value, then we'll make changes to the Basket.cs code to call this UpdateScore( ) method  when a collision happens, finally, we'll update the display of the score.

**Steps: **

1.  Create variable to hold score in GameController.cs
2.  Create a public method: `UpdateScore( int points )`, in the GameController.cs.
3.  Add code to Basket.cs to call the GameController `UpdateScore( )` method.
4.  Add code to update the displayed ScoreText in GameController.cs.
5.  Make sure that the score is reset to 0 when the game is restarted in GameController.cs

###Code Changes in GameController.cs
As detailed in the [GameController section](https://kdoore.gitbooks.io/cs-2335/content/project-1-game-controller.html#object-reference-variables-for-gameobject-components-scoretext-gameovertext), first we need to declare object reference variables that allow us to interact with the ScoreText GameObject.

**Make sure you have added UnityEngine.UI directive**

```java
using UnityEngine.UI;   //Add this additional directive for UI components at the top of the script
```


**Declare Instance Variables For Score**

```java
 private int score; 
 private Text scoreText;

```

**Initialize Variables in Unity Start()**

```java
    // Use this for initialization
    void Start () {
 
   //find text components for score 
    scoreText = GameObject.Find("ScoreText").GetComponent<Text>();
    score = 0; //initialize score value
 
    //other Start code not shown       
    }  //end Start()

```
**Create UpdateScore( ) Method**
This method will be called from the Basket.cs class when a collision has occured, so it must be public.  We'll pass in the points for whatever object we've collided with.


```java
public void UpdateScore(int points)
    { 
score += points;
        scoreText.text = "Score: " + score;
        
    ///Additional code will be added here later
    }

```



