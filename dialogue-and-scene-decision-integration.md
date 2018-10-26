#DialogManager using CustomEvent and ScriptableObject

The link below provides a simple Unity project with a dialog system that you can customize for Project 1. 

[Box.com Unity Package: DialogManager_ Fall 18_v2](https://utdallas.box.com/v/DialogManager-Version2-F18) 

**Includes:** 
    - DialogManager Prefab - Nested UI Components
    - Coroutine for 'dynamic typing-style' dialog reveal
    - Custom UnityEvent: OnDialogClosing - Can be used to trigger opening of other gameObjects
    - Serializable ConversationEntry Class
    - ScriptableObject ConversationList
    - ScriptableObject Factory: (Lior Tal)
    
###Idea 2:  Use public CanvasGroup variable in DialogManager.  
Populate the CanvasGroup variable in the inspector by dragging in the panel that you want to have opened when the dialog is done.  When the dialog is completed, the CloseDialog( ) method checks to see if the CanvasGroup variable refers to a valid object, if so, then it makes that panel active and visible using Utility.ShowCG().

###Add Custom Event Logic to Open Another Panel - OptionPanel or DecisionPanel
One way we we can open a new panel that has our decision buttons, we'll attach the following script to the DecisionPanel that has the scene-transition buttons as child objects. To do this, we'll use the DialogManager that has the custom UnityEvent: OnDialogClosing, shown in code below.

###OpenOptionPanel 
This code will go on a panel that has decision buttons, it listens for the DialogManager OnDialogClosing event. This won't work for having multiple dialogs in one scene.
 
```java

using System.Collections;
using System.Collections.Generic;
using UnityEngine;

//add this script to a panel that will open when the 
//DialogManager's OnPanelClose event happens
public class OpenOptionPanel : MonoBehaviour {

    DialogManager dialogManager;
    CanvasGroup cg;
	// Use this for initialization
void Start () {
        dialogManager = FindObjectOfType<DialogManager>();
        dialogManager.OnDialogClosing.AddListener(OpenPanel);
        cg = GetComponent<CanvasGroup>();
    }
	
	// Update is called once per frame
	public void OpenPanel() {
        Utility.ShowCG(cg);
	}
}

```

- Create a `Custom Unity Event` for the DialogManager that is executed when the dialog is done.
 
- We will write code to invoke the custom event when the dialog is complete.  

- In scene management script, where we have selected to implement this dialogPanel prefab, we'll write a simple method such as:  OpenDecisionPanel.

###DialogManager with Custom UnityEvent OnPanelClosing

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

      
            
   
   
