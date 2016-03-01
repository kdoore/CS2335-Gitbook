#UI-Panel: Toggle Visibility

###Learning Goals:  
- Learn how to execute custom C# script functions using UI-Button GameObjects.
- Learn how to use Layout Component: CanvasGroup attached to a UI-Panel to control visibility and interactivity of a UI-Panel and it's child GameObjects
- Public class methods of class-objects attached to a GameObject can be accessed in the inspector.  We can attach these methods to the onClick event of a Button GameObject
- private class methods can be used for controlling buttons and UI-Panel visibility
- Canvas Group attributes:  Alpha, Interactable, BlocksRaycast determine whether events can be triggered from GameObjects, these can be controlled via custom C# scripts.

###Buttons to control UI Panels.
In the previous section, we also have added a UI-Panel gameObject to our canvas.  Then we used it as a container in the hierarchy to 'hold' our 3 button UI elements. We want to create a different button that can control the visibility of the UI-Panel:  btnPanel and we need to write a custom method in our MenuScript class that will toggle the panel visibility when the user clicks the button. 

The UI Panel can not be directly modified as a component in our C# scripts.  In order to use the panel to control visibility of itself and it's child elements, we need to add a ``Component -> Layout -> CanvasGroup`` to the Panel gameObject.   Here's a link to the Unity Documentation on [CanvasGroup](http://docs.unity3d.com/Manual/class-CanvasGroup.html)

Once we have added the CanvasGroup component to our Panel-UI element, then we can need to write a function that will toggle the panel's alpha value.  We can add this functionality to any C# script file.  

###Scene Load-Event - calls Start()
We need to initialize the value of our `panelHidden` variable and the alpha value within the Start() method.  This way the variables will be re-initialized each time we enter this particular Scene. When a Scene is first loaded, this is when the Start method for every active game object is executed.  

### Toggle Properties: Interactive and BlocksRaycase 

###HideCG, ShowCG Methods

Below is the new code that we've added to MenuScript.cs.  
```
   private CanvasGroup mapCG, startCG;
	
	public void InitializeObjectRefs (){
	
		mapBtn = GameObject.Find ("MapButton").GetComponent<Button> ();
		mapBtn.onClick.AddListener (LoadMapPanel);

         
		startCG = GameObject.Find ("StartPanel").GetComponent<CanvasGroup> ();
		
		mapCG = GameObject.Find ("MapPanel").GetComponent<CanvasGroup> ();

		ShowCG (startCG);
		HideCG (mapCG);

		
	}

	
	private void ShowCG( CanvasGroup cg){
		cg.alpha = 1;
		cg.blocksRaycasts = true;
		cg.interactable = true;
	}

	private void HideCG( CanvasGroup cg){
		cg.alpha = 0;
		cg.blocksRaycasts = false;
		cg.interactable = false;
	}
	
	```

###Call HideCG() from Button onClick()
Now we need to create a button that's not in the btnPanel, this button will control the visibility of the btnPanel.  It will call the MenuScritp.hidePanel() method when it's onClick() event is triggered.



Rather than use the inspector to make connections between our code and the scene gameObjects, we can also create these connections directly in our code using 

```
Private Button sceneBtn;
sceneBtn = GameObject.Find("ButtonInScene").GetComponent<Button>();
sceneBtn.onClick.addListener(myFunction);

void myFunction(){
    //doSomething();
}

``


