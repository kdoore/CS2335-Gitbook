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
	- UI-Text:  Title ( suggestion: put in a Panel)
	- Some Dialog / Backstory
	- 2 Buttons that allow you make a decision about 	how the scene ends
		- no need to have logic for connect to new scenes
		
- **Title (TitlePanel) ** 
	Follow Directions in [HidePanel Script](/project-1-score-and-ui-elements/hidepanel-script.md) Section
	
Suggestion:  Put the TitleText in a UI-Panel. Then, add a CanvasGroup.  The Panel should have a child GameObject that is a Button, it will close the panel.  Add a child GameObject: UI-Text for your Title.  Add the HidePanel.cs script to the parent- UI-Panel.
	

- **Dialog** 
	- You can use the Simple Dialog Prefab if you'd like
	- You can use the DialogManager Package: 
	[Box.com Unity Package: DialogManager_ Fall 18_v2](https://utdallas.box.com/v/DialogManager-Version2-F18)
	
Dialog Requirements:
	-  Have a 'next-type' button to allow player to view successive dialog entries.
	-  When the last dialog is done, close the dialog panel. 
	-  The dialog should present the use with background story and lead up to the decision that ends the scene, where the player will make a decision to go to a different scene.	
	
- **DecisionPanel**
	- Create a UI panel that opens when the dialog is complete, or have the panel hidden behind the dialogPanel.
       - In the Panel, have child 



