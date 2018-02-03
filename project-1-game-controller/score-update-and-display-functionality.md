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

**Overview of Steps: **

1.  Create variable to hold score in GameController.cs
2.  Create a public method: `UpdateScore( int points )`, in the GameController.cs.
3.  Add code to Basket.cs to call the GameController `UpdateScore( )` method.
4.  Add code to update the displayed ScoreText in GameController.cs.
5.  Make sure that the score is reset to 0 when the game is restarted in GameController.cs

###Detailed Steps: Code Changes in GameController.cs
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
 
   //find text components for score so we can modify 
    scoreText = GameObject.Find("ScoreText").GetComponent<Text>();
    score = 0; //initialize score value
 
    //other Start code not shown       
    }  //end Start()

```
**Create UpdateScore( ) Method**
This method will be called from the Basket.cs class when a collision has occured, so it must be public.  We'll pass in the points for whatever object we've collided with.  We'll make additional changes to this code late.

```java
//in GameController.cs
public void UpdateScore(int points)
    { 
    
    score += points; //add new points to current score
    
    scoreText.text = "Score: " + score; //This code sets the text for the ScoreText GameObject
        
    ///Additional code will be added here later to determine if the game is over
    }

```
**Make Changes to StartGame( ) Method**
In the StartGame( ) method, we need to add code to reset the score value back to 0, we also need to update the displayed score by updating scoreText.text


```java

public void StartGame()
    {   
         ///ADD these 2 lines of NEW CODE
        score = 0;  //reset score to 0
        scoreText.text = "Score: " + score;

         
        ///Code below was already here
        gameActive = true; //this is used in other classes to control gameObjects

        //call DropObjects( ) in the appleTree script component
        appleTree.DropObjects(); //start apples dropping

        startButton.SetActive(false); //hide StartButton
     
    }

```

###Apple, Rock, or PickUp Class Code Changes
We need to have points associated with each dropped object.  So, let's add a public instance variable to the classes that represent our dropped objects.  



```java
public class Apple : MonoBehaviour
{
    public int pointValue = 2; //this must be public so we can access it in the Basket.cs class
  
  ///rest of code in this class is unchanged


}

```


###Basket Class Code Changes - Call `UpdateScore( int points)`
In the Basket.cs script file, we need to change the OnCollisionEnter2D( ) event function so that we're updating the score.  

**declare**
First we need to create an object reference to the GameController script component.

```java
 //in Basket.cs

 //declare object reference
 private GameController gameController;
 ```
**initialize**
Then we need to make the connection with the GameController GameObject so we can interact with it.  First we need to find the GameObject named "GameController", then we need to get the GameController script component on that GameObject.  The code below shows how to make that connection


```java
//in Basket.cs
 void Start(){
        gameController = GameObject.Find("GameController").GetComponent<GameController>();
    }
```

**modify**
In the code below, we need to add code to allow us to find out the pointValue associated with each object we've collided with, then we pass those points to the UpdateScore( ) method of the gameController script component.

  
```java
//in Basket.cs

 void OnCollisionEnter2D(Collision2D collision)
    {
        GameObject collidedWith = collision.gameObject; //get  a reference to the gameObject 
    
       
        if(collidedWith.tag == "Apple"){
            //get a reference to the Apple script to find out how many points this object is worth.
            Apple apple = collidedWith.GetComponent<Apple>(); //create a connection with the Apple script component on the apple 
            //so we can get the pointValue.
            //then pass those points to UpdateScore( )
            gameController.UpdateScore(apple.pointValue);
    
            Destroy(collidedWith);
        }else if(collidedWith.tag =="Rock")
  {
            //same as above, we want to pass the rock's pointValue to the UpdateScore( ) method.
            Rock rock = collidedWith.GetComponent<Rock>();
            gameController.UpdateScore(rock.pointValue);
            Destroy(collidedWith);
        }
    }


```

###Verify if Changes Work
When you play the scene, after you have pressed the StartGame Button, does the ScoreText get updated, so the value is modified as you collide with objects?  The score should increase when you collide with positive objects, it should decrease when you collide with negative objects.  

###Debugging 

1. When you run the game, do you get any errors in the console?    If you have any errors, see if you can identify the line in your scripts that's causing the error.  You must eliminate all error messages before you start any other debugging steps.

 2.  If the ScoreText isn't changing value when the game runs, stop the game, change the starting text for the ScoreText, so that it starts with no value, 'Score:'.  This should be modified when you hit the StartButton, because that is the first place where we try to modify it:  ` scoreText.text = "Score: " + score; ` . So, it should change to display 'Score: 0' after this StartButton code is executed. 


3. Where can you add Debug.Log( ) statements to your code to give yourself more info about what's not working correctly?   

The next place to add a debug statement is in the Basket.cs class, within the OnCollisionEnter2D, we would want to see the point value associated with the apple or rock objects, so we should add this Debug line of code between these lines.


```java

Apple apple = collidedWith.GetComponent<Apple>();
Debug.Log("apple points: " + apple.pointValue); //Add this debug statement
gameController.UpdateScore(apple.pointValue);
```


            


