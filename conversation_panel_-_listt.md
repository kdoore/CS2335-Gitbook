# Conversation Panel - List  

In order to add a simple text panel that can allow for clicking through a list of dialog, we can take a basic approach of using a List of strings, where a button can be used to move to the next dialog text.

In the game scene, we'll need a UI-Panel, a UI-Text element, and a UI-Button.  Then we'll create a script that can be attached to a gameObject like the canvas, in the scene.  Below are code snippets to create this functionality.

```C#
using UnityEngine;
using System.Collections;
using System.Collections.Generic;
using UnityEngine.UI;

public class BeginSceneDialogManager : MonoBehaviour
{
    Button titleBtn, nextBtn;
	CanvasGroup cg1;
	List<string> dialogList;
	Text titleTxt;
	int currentDialog;

}

void Start ()
	{
		currentDialog = 0;
		cg1 = GameObject.Find ("ButtonPanel").GetComponent<CanvasGroup> ();
		Utility.hideCG (cg1);
		titleBtn = GameObject.Find ("TitleButton").GetComponent<Button> ();
		titleBtn.onClick.AddListener (ShowBtnPanel);

		nextBtn = GameObject.Find ("NextButton").GetComponent<Button> ();
		nextBtn.onClick.AddListener (ShowNextDialog);

		dialogList = new List<string> ();
		PopulateDialogList ();
		titleTxt = GameObject.Find ("TitleText").GetComponent<Text> ();
		titleTxt.text = dialogList [currentDialog];

	}

	void ShowNextDialog ()
	{
		currentDialog++;
		Debug.Log ("Just incremented counter " + currentDialog);
		if (currentDialog < dialogList.Count) {
			titleTxt.text = dialogList [currentDialog];
		}

	}

	void PopulateDialogList ()
	{
		dialogList.Add ("Welcome to the adventure");
		dialogList.Add ("Do you have any money? ");
		dialogList.Add ("I'm also happy to trade");
	}
    
    public void ShowBtnPanel ()
	{
		CanvasGroup cgTitlePanel = GameObject.Find ("TitlePanel").GetComponent<CanvasGroup> ();
		Utility.showCG (cg1);
		Utility.hideCG (cgTitlePanel);
	}

```