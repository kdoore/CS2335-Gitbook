# StateManager

In Project 2, we have created the StateManager class to manage which state our application is in.  The StateManager inherits from MonoBehavior, so this class can be attached to an empty gameObject: GameManager, and Unity will execute the Start(), Update(), and other Unity event hook functions.  

### Singleton Design Pattern
We want to insure that this class is only created once, otherwise everytime it is created, the constructor initialization would return us to whatever we set as the initial state.  Instead, we want to use one instance of the StateManager class throughout the lifetime of our application. In order to insure that 1 single instance is created when our game is started, and that no other instances are ever created, we can create a special static class reference variable:``StateManager instanceRef; ``, and 

###Object Persistence 
Unity proivdes a special method that we can use to insure the GameManager and attached script component: StateManager are not destroyed when execution jumps to a new scene: ``DontDestroyOnLoad(gameObject)``

###Panel-Canvas Group: Modify Attributes on Button Click
In the code below, we add a clickEvent Listener, so if the StartButton is clicked, this showStoryPanel() method will be called.  In the showStoryPanel(), we want to hide the canvas group: cg that 

```java
//find StartButton GameObject - add clickEvent Listener
		startButton=GameObject.Find ("StartButton").GetComponent<Button>();
		startButton.onClick.AddListener(showStoryPanel1);	
```


```java
    /// <summary>
	///Called from main panel button
	///hides and inactivates the main panel 
	///then shows and activates  StoryPanel1
	/// </summary>
	public void showStoryPanel1(){
		Debug.Log ("StartButton was clicked");
		//hide mainPanel
		cg.alpha=0;  //set the mainPanel to be invisible
		cg.interactable=false;  //turn off interactivity for the invisible mainPanel
		cg.blocksRaycasts=false;
		//show storyPanel1
		cg1.alpha=1;
		cg1.interactable=true;
		cg1.blocksRaycasts=true;
	}
	```