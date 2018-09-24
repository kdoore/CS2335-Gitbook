#Dialog and Scene-Transition Integration

The link below provides a simple Unity project with a dialog system that you can customize for Project 1. 

[Box.com Unity Package: DialogManager_ Fall 18_v1](https://utdallas.box.com/s/7c2e1nhk99r5kttb9e0ee2kagcpoxmq1)

[Box.com Unity Package: DialogManager_ Fall 18_v2](https://utdallas.box.com/v/DialogManager-Version2-F18) 

**Includes:** 
    - DialogManager Prefab - Nested UI Components
    - Coroutine for 'dynamic typing-style' dialog reveal
    - Custom UnityEvent: OnDialogClosing - Can be used to trigger opening of other gameObjects
    - Serializable ConversationEntry Class
    - ScriptableObject ConversationList
    - ScriptableObject Factory: (Lior Tal)
    

###How to Integrate Dialog with Scene-Transition logic:
In order for this to work as a prefab, we have written the code in a general way so it'll work in any scene. In other words,  we don't want any scene-specific logic in any of the code for this script. 

In Project 2, we will want all scene-transition logic to be contained in a StateX.cs (which we haven't created for Project 1). 

So how can we integrate dialog display with the buttons for scene-transition decisions? How do we get the dialog panel (prefab) to open a new panel, with buttons that can be used for scene transition?

- **Issue: we want the general behavior of a prefab, but we want to integrate event-logic to open a panel for the scene-transition decision logic. ** 

   
###Idea 1. Hide the Buttons Behind the Panel
  The most obvious, simple solution is to hide scene-transition buttons beneath the dialog panel, so they are visible once the dialog panel is closed. In this case, the dialog panel should have it's color set to full opacity, and it might need be be larger than necessary, in order to hide 2 buttons behind the panel.    


###Idea 2:  Use public CanvasGroup variable in DialogManager.  
Populate the CanvasGroup variable in the inspector by dragging in the panel that you want to have opened when the dialog is done.  When the dialog is completed, the CloseDialog( ) method checks to see if the CanvasGroup variable refers to a valid object, if so, then it makes that panel active and visible using Utility.ShowCG().


```java
public class DialogManager : MonoBehaviour {
 
 public CanvasGroup nextPanelToShow; //populate in inspector
 
 public void CloseDialog()
    {
        Utility.HideCG(dialogPanelCG);
        Debug.Log("Closing Dialog");
        if(nextPanelToShow != null){
           Utility.ShowCG(nextPanelToShow);
        }
    }

```



###Idea 3: Add Custom Event Logic to Open Another Panel
Using slightly more complex logic, we can open a new panel that has the scene-transition buttons as child objects. To do this, we must first make our dialog-panel generate an event that can be used to trigger opening of this buttonPanel, when the dialog is complete. 



public CanvasGroup nextPanelToShow;



 Below are the steps required to do this:  

 - Put the Scene-decision buttons in a UI-panel that has an attached CanvasGroup component, so that panel can have it's visibility set to true when the dialog is complete.  

- Create a `Custom Unity Event` for the DialogPanel.
 
- We will write code to invoke the custom event when the dialog is complete.  

- In scene management script, where we have selected to implement this dialogPanel prefab, we'll write a simple method such as:  OpenDecisionPanel.

###Custom UnityEvent OnPanelClosing

```java

using System.Collections;
using System.Collections.Generic;
using UnityEngine;
using UnityEngine.UI;
using UnityEngine.Events; //must include this statement

public class DialogManager : MonoBehaviour {

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
    
            CloseDialog();
        }

    }
    
    public void CloseDialog()
    {
        Utility.HideCG(dialogPanelCG);
        if(OnDialogClosing != null){ //check to make sure there are listeners
            Debug.Log("Invoking OnDialogClosing");
            OnDialogClosing.Invoke(); //Invoke all event listener - methods to open another panel
        }
    }
	
}

```
###How to Use the Custom Unity Event
//Decision Buttons have been placed into a panel and we want to open this DecisionPanel when the DialogManager's onDialogClosing event has happened.

The DialogManager script now includes a custom UnityEvent: onDialogClosing,  so, we just need to write a simple method (OpenDecisionPanel) that can be executed when that event occurs.  

The DecisionPanel is a UI-panel that contains the 2 buttons that allow users to decide which scene to go to next.  The DecisionPanel must have an attached CanvasGroup component. This script: DecisionScript_TextScene.cs should also be attached to the DecisionPanel.


```java
//in DecisionScript_TestScene for Project 1
  
    private DialogController dialogController;
    private CanvasGroup decisionPanelCG;

  void Start ()
	{
    //other code here
    
    decisionPanelCG = GetComponent<CanvasGroup>();
    Utility.HideCG(decisionPanelCG); //make sure to hide at Start 
    
    dialogManager = GameObject.Find("DialogPanel1").GetComponent<DialogManager>();  //find the DialogPanel 
    dialogManager.onDialogClosing.AddListener(OpenDecisionPanel) ; //specify the method to be executed when the event happens
   }
   
   
   //method to be executed when onPanelClosing event has happened
    public void OpenDecisionPanel(){
        Utility.ShowCG(decisionPanelCG); 
    }
 } 
```

      
            
   
   
