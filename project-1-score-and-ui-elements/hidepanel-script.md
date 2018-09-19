#HidePanel, Universal Script

Suggestion:  You can write a simple script that can be used on any UI panel to hide the panel when the first child Button is clicked

###Steps:
Create a UI panel: TitlePanel to act as a container for the Title UI-text and startButton objects.
 
 
 
	- Create a StartButton as a child of the TitlePanel
	- Create a Title UI-text as a child of the TitlePanel
	- Add a CanvasGroup to the TitlePanel
	
![](/assets/Screen Shot 2018-09-19 at 1.59.01 PM.png)	
	![](/assets/Screen Shot 2018-09-19 at 1.59.26 PM.png)
	
	- Create a customScript: HidePanel.cs - This can be a re-usable script.
	- Attach the HidePanel script as a component to the TitlePanel
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
public void HideThisPanel(){
	   Utility.HideCG( canvasGroup );
	}
```
	- Add the HideThisPanel( ) method as a listener to the Button's onClick event:
	
		`closeBtn.onClick.AddListener( HideThisPanel );`

##Complete Script
Here's the complete script

```java

using System.Collections;
using System.Collections.Generic;
using UnityEngine;
using UnityEngine.UI;

/// <summary>
/// Hide panel.
/// Requires a CanvasGroup component
/// Requires a childObject: Button 
/// </summary>
public class HidePanel : MonoBehaviour {

    CanvasGroup canvasGroup; //on the Panel
    Button closeBtn;   //child gameObject is a Button
	// Use this for initialization
	void Start () {
        canvasGroup = GetComponent<CanvasGroup>();
        closeBtn = GetComponentInChildren<Button>();
        closeBtn.onClick.AddListener(HideThisPanel);
        Utility.ShowCG(canvasGroup); //shows initially
	}
	
    public void HideThisPanel(){
        Utility.HideCG(canvasGroup);
    }
}

```


