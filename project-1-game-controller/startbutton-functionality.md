###StartButton Functionality

In this section, we'll take a look at the functionality of the StartButton in the Scene and the Scripts and other gameObjects that have behaviors that connected with this gameObject.

The StartButton is used to start the game, it is used to call the custom StartGame( ) method in the GameController class. Once the StartButton has been clicked, objects should start falling from the AppleTree, and the StartButton should be hidden. When the StartButton is clicked, the GameController.gameActive  flag variable should be set to true.  

###Steps to Create / Configure The StartButton GameObject
1. Add a UI-Button to the Scene
2. Rename the Button to StartButton
3. Set Button colors: Highlight and Normal colors should be different.
4. Set the Text GameObject to say `Start Game`
5. Set the Anchors and Position of the StartButton, so that it's anchored and moved to the bottom-center of the screen.  
6. Once the anchors are set for the Button, the move tool can be used to move the Button away from the bottom edge.
7. On the OnClick section of the Button's script component, select the GameController gameObject, then select the StartGame() method. This method will be executed when the StartButton is clicked.

###Animation to Create and Configure StartButton GameObject

![](http://g.recordit.co/j0q1PTchAr.gif)

###Code for the StartButton is in the GameController Script



```java
//GameController.cs

using UnityEngine.UI;  //must be added at top of script

//declare object reference variable to connect with the StartButton gameObject

 private GameObject  startButton; 
 public bool gameActive; //used to control start/stop behavior in many other classes
 
 private AppleTree appleTree //allows us to interact with our custom AppleTree script on the AppleTree GameObject
 
 void Start () {
        //find all gameObjects that will be activated / deactivated 
       startButton = GameObject.Find("StartButton"); //initialize object reference to create connection with Scene GameObject
      
       //get a reference to the AppleTree script component
       appleTree = GameObject.Find("AppleTree").GetComponent<AppleTree>();
      
       gameActive = false;   //initialize to false
      
      //other code in Start() is not shown
      }
      
      
    /// <summary>
    /// Starts the game.
    /// this function Must be declared as public 
    /// function to be called by StartButton, 
    /// This will be added to the OnClick event in the inspector for the StartButton
    /// </summary>
    public void StartGame()
    {
        score = 0;  //reset score to 0
        //scoreText.text = "Score: " + score;  //Use once we have ScoreText GameObject etc

        gameActive = true; //this is used in other classes to control gameObjects

        appleTree.DropObjects(); //start apples dropping to start the game
        
        //Invoke("appleTree.DropObjects", 2f);  //could also use this to start apples dropping with 2 second delay.

        startButton.SetActive(false); //hide StartButton
        // gameOverPanel.SetActive(false); // don't use this code until we have created the GameOverPanel etc
    }
      
      
```

