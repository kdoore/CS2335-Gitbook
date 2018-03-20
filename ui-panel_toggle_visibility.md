#UI-Panel: Toggle Visibility

The discussion below provides an overview of how to work with UI gameObjects, when you want to toggle the visibility of these objects.  This is a general overview, along with some chunks of example code, but for specific examples of using these concepts, look at the following gitbook sections: [Simple Dialog Prefab](/simple-dialog-prefab.md), [Dialogue and Scene-Decision Integration](/dialogue-and-scene-decision-integration.md).  Finally, code to show / hide UI elements is used frequently, so it will be added to a custom static class: [Utility](/utility_-_static_class.md), so it is easier to use.

##Learning Goals:
### Overview of Using CanvasGroup to Show / Hide UI-Elements:  
- Learn how to execute custom C# script functions using UI-Button GameObjects.
- Learn how to use Layout Component: CanvasGroup attached to a UI-Panel to control visibility and interactivity of a UI-Panel and it's child GameObjects
- Canvas Group attributes:  Alpha, Interactable, BlocksRaycast determine whether events can be triggered from GameObjects, these can be controlled via custom C# scripts.

###Buttons to control UI Panels.
If we add a UI-Panel gameObject to our canvas.  We can use it as a container in the hierarchy to 'hold' our other UI elements. We can create a button: `HideButton` to control the visibility of this UI-Panel:  `TextPanel` and we need to write a custom method in our Script class that will toggle the panel visibility when the user clicks the button. 

###Layout Component: CanvasGroup controls UI-Transparancy
A UI Panel often is used as a container for holding other UI elements.  In order to use the panel to control visibility of itself and it's child elements, we can add a ``Component -> Layout -> CanvasGroup`` to the Panel gameObject.   Here's a link to the Unity Documentation on [CanvasGroup](http://docs.unity3d.com/Manual/class-CanvasGroup.html)

Once we have added the CanvasGroup component to our Panel-UI element, then we can need to write a function that will toggle the panel's alpha value, interactibility,and blocksRaycast properties.  We can add this functionality to any C# script file. We can put this code: HideCG( cg ), ShowCG( cg ) in a custom static class: Utility.  

###Scene Load-Event - Initialize Object References
We need to initialize the value of our object references within the Unity Start() method.  This way the object references will be re-initialized each time we enter this particular Scene. 

### Find GameObject Components: Button, CanvasGroup 

Below is the code that we've added to  a new script: `PanelVisiblity.cs `, or this code can be added to one of the stateX.cs scripts in our projects.
We need object-reference variables for our UI panel's CanvasGroup component: `panelCG`, and the Button component: `hideBtn`  

```java
   //Declare Object Reference Variables for UI Components
   private Button hideBtn;
   private CanvasGroup  panelCG;
	
  void Start (){
  	//CanasGroup components on Panel gameObjects
	panelCG = GameObject.Find ("TextPanel").GetComponent<CanvasGroup>();
  
	    ///hideButton is used to hide the TextPanel 
	hideBtn = GameObject.Find ("HideButton").GetComponent<Button>();
	hideBtn.onClick.AddListener (HidePanel);  //method called when hideBtn is clicked	
	
	ShowCG( panelCG); //make sure panel is visible 
	} //end Start
	
```
###Call HidePanel() from Button onClick()
In the Unity Scene, we need to have a button that's a child of the TextPanel.  This button will control the visibility of the TextPanel.  It will call the HidePanel() method when it's onClick() event is triggered. 


	
### HideCG, ShowCG Methods
The code below shows the HidePanel method, this is executed when the hideButton onClick event is invoked. Within that method, we call HideCG( cg ) method where we actually toggle the properties.



```java
public void HidePanel(){
	// call the HideCG function
	HideCG( panelCG); // hides the panel
	
	//or, if you have a Utility class:
	//Utility.HideCG (PanelCG);
}


	//Modifies CanvasGroup component properties to make visible
	public void ShowCG( CanvasGroup cg){
		cg.alpha = 1;
		cg.blocksRaycasts = true;
		cg.interactable = true;
	}

    //Modifies CanvasGroup component properties to hide
	public void HideCG( CanvasGroup cg){
		cg.alpha = 0;
		cg.blocksRaycasts = false;
		cg.interactable = false;
	}
	
```

###Button onClick Event - Event-Handler Methods
It would have been nice if we could just call HideCG( panelCG ) directly from the button's onClick event...but any method used for a UI-Button's onClick event should not have any input parameters, but HideCG( cg ) takes a CanvasGroup input parameter. So we need a helper method: HidePanel( ), which can be invoked by the button's onClick event.
`
