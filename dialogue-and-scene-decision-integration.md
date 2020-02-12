#DialogManager using CustomEvent and ScriptableObject

###OpenPanelOnDialogClosing
This script will go on a panel that has decision buttons, it listens for the DialogManager OnDialogClosing event. This can be used in all scenes.  This will only when there's only one dialog in the scene.  It will open the panel with Buttons to change scene.  The Button logic will be in the SceneXState.cs file for the corresponding scene.
 
```java

using System.Collections;
using System.Collections.Generic;
using UnityEngine;

//add this script to a panel that will open when the 
//DialogManager's OnPanelClose event happens
public class OpenPanelOnDialogClosing : MonoBehaviour {
public DialogManager dialogManager; //set in Inspector
    CanvasGroup cg; // cg on same gameObject as current script
	// Use this for initialization
void Start () {
                dialogManager.OnDialogClosing.AddListener(OpenPanel);
        cg = GetComponent<CanvasGroup>();
    }
	
	//Displays using cg on current gameObject
	public void OpenPanel() {
        Utility.ShowCG(cg);
	}
}

```
###DialogManager with Custom UnityEvent OnPanelClosing


- Create a `Custom Unity Event` for the DialogManager that is executed when the dialog is done.
 
- We will write code to invoke the custom event when the dialog is complete.  

- In scene management script, where we have selected to implement this dialogPanel prefab, we'll write a simple method such as:  OpenDecisionPanel.

###DialogManager w/Custom Event Code

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

      
            
   
   
