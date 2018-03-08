#Dialog and Scene-Transition Integration

###How to Integrate Dialog with Scene-Transition logic:
In order for this to work as a prefab, we have written the code in a general way so it'll work in any scene. In other words,  we don't want any scene-specific logic in any of the code for this script. 

In addition, we want all scene-transition logic to be contained in the StateX.cs files. So how can we integrate dialog display with the buttons for scene-transition decisions? How do we get the dialog panel (prefab) to include a final display state, with buttons that can be used for scene transition?

- **Issue: we want the general behavior of a prefab, but we want to integrate scene-transition decision logic. ** 

There are a few different issues here:
 
  - **Issues:**

   - We don't want to add scene specific logic to a prefab, otherwise we can't use it anywhere. 
   - We want all scene-transition logic in the StateX.cs files, not attached to a panel gameObject
   - We don't have a way to let the StateX file know that the dialog has finished, so that scene-transition buttons can be displayed after the dialog has finished.
   
###Idea 1. Hide the Buttons Behind the Panel
  The most obvious, simple solution is to hide scene-transition buttons beneath the dialog panel, so they are visible once the dialog panel is hidden.  

###Idea 2: Add Logic to Open Another Panel
For more complex logic, we can open a new panel from the dialogPrefab, when the dialog is complete.  What is the best way to do this?  

 - Put the Scene-decision Buttons in a Panel with a CanvasGroup component, so that panel can have it's visibility set to true when the dialog is complete.

 -  How can we tel the dialog is complete?
 
 -  We can write a new method within the corresponding sceneState.cs file, and have that method executed by the onClick event of the panel's next button
 -  The dialog is only finished the last time that the next button is clicked.  So, we could create a variable:  boolean dialogComplete = false; //
We'd set this variable to true when the dialog queue is empty, we'd need to check this everytime in the new method OpenDecisionPanel in the SceneState.cs script.
 - We can write a **custom UnityEvent**, that we'll invoke when the dialog is complete.  The OpenDecisionPanel method will be a listener to this event.  **This is the best option - **

###Custom UnityEvent OnPanelClosing

```java

using System.Collections;
using System.Collections.Generic;
using UnityEngine;
using UnityEngine.UI;
using UnityEngine.Events; //must include this statement

public class DialogController : MonoBehaviour {

    public UnityEvent onPanelClosing; //define the custom event

    CanvasGroup cg;
    Button nextBtn;
    Text dialogText;

    [TextArea]
    public List<string> dialogList = new List<string>();
    private Queue<string> queue = new Queue<string>(); //declare, initialize

	// Use this for initialization
	void Start () {
        //if event hasn't been initialized
        if (onPanelClosing == null)
        {
            onPanelClosing = new UnityEvent();
        }

        cg = GetComponent<CanvasGroup>();

        nextBtn = GetComponentInChildren<Button>();
        nextBtn.onClick.AddListener(GetNextDialog);

        dialogText = GetComponentInChildren<Text>();

        //put each string in the list into the queue
        foreach(string curString in dialogList){
            queue.Enqueue(curString);
        }

        if(queue.Count !=0){
            dialogText.text = queue.Dequeue();
        }
    
        //make sure panel is showing
        Utility.ShowCG(cg);
	}

    public void GetNextDialog(){
        if (queue.Count != 0)
        {
            dialogText.text = queue.Dequeue();
        }else{  //No more dialog in the queue
            Utility.HideCG(cg); //hide panel
    
            //check to see if anyone is listening
            if (onPanelClosing != null)
            {
                onPanelClosing.Invoke(); //invoke event        
            }
        }

    }
	
}

```
###How to Use the Custom Unity Event
//Code in BeginState, where we've put buttons into a panel and we want to open this Decision Panel when the dialogPanel event has happened.

Essentailly we'll be able to treat the dialogPanel as if it was a button, because the DialogController script now includes a UnityEvent: onPanelClosing.  So, we just need to write a simple method (OpenBtnPanel) that can get executed when that event occurs.  

The OpenBtnPanel just shows a panel that contains the 2 buttons that allow users to decide which scene to go to next.  The ButtonPanel must have an attached CanvasGroup component.



```java
//in BeginState.cs
  
    private DialogController dialogController;
    private CanvasGroup btnPanelCG;

   public void InitializeObjectRefs ()
	{
    //other code here
    
    btnPanelCG = GameObject.Find("ButtonPanel").GetComponent<CanvasGroup>();
    Utility.HideCG(btnPanelCG); //make sure to hide 
    dialogController = GameObject.Find("DialogPanel").GetComponent<DialogController>();  //find the DialogPanel 
    dialogController.onPanelClosing.AddListener(OpenBtnPanel) ; //specify the method to be executed when the event happens
   }
   
   
   //method to be exectued when onPanelClosing event has happened
    public void OpenBtnPanel(){
        Utility.ShowCG(btnPanelCG); 
    }
    
    //other code in BeginState.cs
    
    } //end BeginState.cs class	
```

      
            
   
   
