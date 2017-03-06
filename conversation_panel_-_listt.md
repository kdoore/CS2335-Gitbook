# Conversation Panel - List  

In order to add a simple text panel that can allow for clicking through a list of dialog, we can take a basic approach of using a List of strings, where a button can be used to move to the next dialog text.

In the game scene, we'll need a UI-Panel, a UI-Text element, and a UI-Button.  Then we'll create a script that can be attached to a gameObject like the canvas, in the scene.  Below are code snippets to create this functionality.

###Required GameObjects for this code to work:

UI-Panels - with canvasGroup component:  TitlePanel, DialogPanel

Within the Panel Game-objects, the following gameObjects as children:

- **TitlePanel: _UI-Panel_ **
	- TitleButton  _UI-Button_
	
- ** DialogPanel: _UI-Panel_ **
	- NextButton  _UI-Button_
	- DialogText  _UI-Text_

Also, you need to have our custom [Utility Class](https://kdoore.gitbooks.io/cs-2335/content/utility_-_static_class.html#utility---static-class) included in your project assets, so you can use the static methods: 

```java

public static void ShowCG( CanvasGroup cg)
public static void HideCG( CanvasGroup cg)
```

###DialogManager Code

```C#
//updated spring 17

using System.Collections;
using System.Collections.Generic;
using UnityEngine;
using UnityEngine.UI;

public class DialogManager : MonoBehaviour
{

	List<string> dialogList;

	CanvasGroup titleCG, dialogCG;
	Button titleBtn, nextBtn;
	Text dialogTxt;
	int dialogCounter;

	// Use this for initialization
	void Start ()
	{
		dialogCounter = 0;

		dialogList = new List<string> ();
		dialogList.Add ("Hello");
		dialogList.Add ("What's new");
		dialogList.Add ("Not Much");
		dialogList.Add ("Good bye");

		titleCG = GameObject.Find ("TitlePanel").GetComponent<CanvasGroup> ();
		dialogCG = GameObject.Find ("DialogPanel").GetComponent<CanvasGroup> ();

		Utility.ShowCG (titleCG);
		Utility.HideCG (dialogCG);

		titleBtn = GameObject.Find ("TitleButton").GetComponent<Button> ();
		nextBtn = GameObject.Find ("NextButton").GetComponent<Button> ();
		titleBtn.onClick.AddListener (OpenDialogPanel);
		nextBtn.onClick.AddListener (ShowNextDialog);


		dialogTxt = GameObject.Find ("DialogText").GetComponent<Text> ();
		dialogTxt.text = dialogList [dialogCounter];
		dialogCounter++;
	}

	public void OpenDialogPanel ()
	{
		Utility.HideCG (titleCG);
		Utility.ShowCG (dialogCG);
	}

	public void ShowNextDialog ()
	{
		
		if (dialogCounter < dialogList.Count) {
			dialogTxt.text = dialogList [dialogCounter];
		}
		dialogCounter++;
	}


}
```