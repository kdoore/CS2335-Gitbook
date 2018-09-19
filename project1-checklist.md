#Project 1:  CheckList

For Project 1, follow these steps to complete the **Unity Project** requirements:  

- Create a 2D Unity Project
- **Create 5 Scenes**, each with a background image
	- Configure: Canvas Rendering-Mode:  Screen-Space Camera
	- Add a Canvas GameObject
	- Background Image:  
		- Add a UI-Panel
		- Select a sprite to your background image.
		- Adjust the sprite color's alpha so it's 100%.
		- Make sure this is the top item in the canvas hierarchy, so it is behind all other UI elements

- **Create Scene1 GameObjects**
	- UI-Text:  Title
	- Some Dialog / Backstory
	- 2 Buttons that allow you make a decision about 	how the scene ends
		- no need to have logic for connect to new scenes
		
- **Title (TitlePanel) ** 
	- Suggestions:
	- Create a UI panel: TitlePanel to act as a container for the Title UI-text and startButton objects.
	- Create a StartButton as a child of the TitlePanel
	- Create a Title UI-text as a child of the TitlePanel
	- Add a CanvasGroup to the TitlePanel
	- Create a customScript: ClosePanel.cs - This can be a re-usable script.
	- Attach the ClosePanel script as a component to the TitlePanel
	- Declare object-reference variables for the Button and CanvasGroup components.
		

```java
		Button closeBtn;
		CanvasGroup canvasGroup;
```


	- In the script - Start( ):
		- Find the CanvasGroup:
			
				`canvasGroup = GetComponent<CanvasGroup>();`
		- Find the Button as a child component of the Panel		
			
				`closeBtn = GetComponentInChildren<Button>();`
		
	- Write a function that can be executed when the button is clicked: 
	
	

```java
public void HidePanel(){
	   Utility.HideCG( canvasGroup );
	}
```
	- Add the ClosePanel( ) method as a listener to the Button's onClick event:
	
			`closeBtn.onClick.AddListener( ClosePanel );`


- **Dialog** 
	- You can use the Simple Dialog Prefab if you'd like
	- You can use the DialogManager Package: 
	[Box.com Unity Package: DialogManager_ Fall 18_v2](https://utdallas.box.com/v/DialogManager-Version2-F18)
	
Dialog Requirements:
	-  Have a 'next-type' button to allow player to view successive dialog entries.
	-  When the last dialog is done, close the dialog panel. 
	-  The dialog should present the use with background story and lead up to the decision that ends the scene, where the player will make a decision to go to a different scene.	
	
- **DecisionPanel**

