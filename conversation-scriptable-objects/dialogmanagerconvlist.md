#DialogManager for ConversationList

See - [Conversation ScriptableObjects](/conversation-scriptable-objects.md)

###Inspector Images
The images below show the Inspector panel configuration for the DialogPanel and Decision panels. ** Note that the SpeakerImg is the 5th child image which has index [4]**, because all panels and buttons also have an image component.
 
![](/assets/Screen Shot 2019-03-11 at 2.13.59 PM.png)
![](/assets/Screen Shot 2019-03-11 at 2.14.38 PM.png)

###NextPanelToOpen
The DialogManager script has a public reference variable that can be set in the Inspector allows for the nextPanelToOpen to be populated. After all of the dialog entries has been displayed, the dialogPanel is closed, and then the nextPanelToOpen will be opened.  If not  populated, no error will occur because the code first checks to see if that variable contains a valid object reference (memory address). 

Add the linked script to the DecisionPanel so it will stay closed at the start of the scene, if desired [Hide_Show_Panel Script ](/conversation-scriptable-objects/dialogmanagerconvlist/hideshow-panel-script.md)


 ###OpenDialogBtn
 The OpenDialogBtn public reference in the Inspector should be populated if a button will be used to open the dialog.  Otherwise, the ShowOnStart checkbox in the Inspector should be selected, it will make the panel visible at the beginning of the scene. 
 
** For our example, we'll set OpenDialogBtn to the TitlePanel's StartDialogBtn. **

   
**Includes public CanvasGroup nextPanelToOpen;**
```
 if(nextPanelToOpenCG != null){
        Utility.ShowCG(nextPanelToOpenCG);
        }
```

###DialogManager - Without Custom Unity Event Full Code

```java
using System.Collections;
using System.Collections.Generic;
using UnityEngine;
using UnityEngine.UI;

/// <summary>
/// Dialog manager.
/// Put this script on a panel with canvasGroup, 
/// Important:  remove SpriteRenderer from Panel
/// required child objects: dialogText, speakerName, nextDialogBtn
/// </summary>
public class DialogManager : MonoBehaviour {

    public CanvasGroup nextPanelToOpenCG;  //next panel to open, set in Inspector
    public Button openDialogBtn; //Button that will open Dialog
    public bool showOnStart = false;

    private Button nextBtn;
    private CanvasGroup dialogPanelCG;
    private Text dialogText, speakerName; //speakerName;
    private Image speakerImage;
  

    public ConversationList convList; //attach scriptable object in Inspector
    private Queue<ConversationEntry> conversationsQueue = new Queue<ConversationEntry>();
   
    // Use this for initialization
    void Start () {

      
        dialogPanelCG = GetComponent<CanvasGroup>(); //used to show/hide panel
       
        Text[] textChildren = GetComponentsInChildren<Text>();
        Image[] imageChildren = GetComponentsInChildren<Image>();

        dialogText = textChildren[0];
        speakerName = textChildren[2];
        speakerImage = imageChildren[4]; //fifth child image - in SpeakerPanel

        if (openDialogBtn != null ) //if opening dialog with a Button, Populate OpenDialogButton in the Inspector 
        {
            openDialogBtn.onClick.AddListener(ShowDialogPanel);
        }

        nextBtn = GetComponentInChildren<Button>();
        nextBtn.onClick.AddListener(GetNextDialog);

        InitializeDialog();
        //checkbox that can be set in inspector, if checked, then this is not exected
        if (!showOnStart)
        {
            Utility.HideCG(dialogPanelCG);
        }
        else{ //if showing on scene load, get first Dialog 
            Utility.ShowCG(dialogPanelCG);
        }
    }


    void InitializeDialog()
    {
        foreach (ConversationEntry item in convList.Conversation)
        {
            conversationsQueue.Enqueue(item); //put each string -item in the queue
        }
        GetNextDialog();  //get first item
    }


    /// <summary>
    /// Opens the dialog.
    /// this method is called if there is an 
    /// openDialog button set in the Inspector
    /// otherwise, select checkbox showOnStart 
    /// </summary>
    public void ShowDialogPanel()
    {
        Utility.ShowCG(dialogPanelCG);
        openDialogBtn.gameObject.SetActive(false);
    }


    /// <summary>
    /// Nexts the dialog.
    /// Sets UI elements: speakerName, speakerImage, dialogText
    /// </summary>
    /// <returns><c>true</c>, if dialog there is more dialog, <c>false</c> otherwise.</returns>
    public void GetNextDialog()
    {   
        if (conversationsQueue.Count >0)
        {   ConversationEntry item = conversationsQueue.Dequeue();
            speakerName.text = item.speakerName;
            speakerImage.sprite = item.speakerImg;

            StopAllCoroutines();
            string curSentence = item.dialogTxt;
            StartCoroutine(TypeSentence(curSentence));
        }
        else //no more conversations
        {
            Utility.HideCG(dialogPanelCG); // close panel
            if (nextPanelToOpenCG != null) //check to see if valid gameObject was set in inspector
            {
                Utility.ShowCG(nextPanelToOpenCG);
            } 
        }
    
    }

    //this allows single characters to be added to look like typed text
    IEnumerator TypeSentence(string sentence)
    {
        dialogText.text = ""; //clear previous sentance
        foreach (char letter in sentence.ToCharArray())
        {
            dialogText.text += letter;
            yield return new WaitForSeconds(0.05f); ;
        }
    }
} // end class

```

##Configure Other Items Used for DialogPrefabConvList

###Hide_Show_Panel, CanvasGroup
![](/assets/Screen Shot 2019-03-06 at 3.25.54 PM.png)
This script component will be added to any panel that we'd like to show or hide during gameplay.  

###TitlePanel - Prefab
![](/assets/Screen Shot 2019-03-06 at 2.26.55 PM.png)


**TitlePanel:** This is a UI-Panel with children:
- **Children:**
- UI-Text: TitleText
- UI_Button: StartDialogBtn

- **Attach CanavasGroup **
- **Attach Hide_Show_Panel.cs **
The Hide_Show_Panel script will make sure the panel shows at start, and is hidden when the StartDialogBtn is clicked. 

- Configuration for **Hide_Show_Panel.cs on TitlePanel: **
- ShowOnStart checked as True
- CloseButton: StartDialogBtn
- OpenButton: empty

###DecisionPanel - Prefab
![](/assets/Screen Shot 2019-03-06 at 2.27.05 PM.png)

**DecisionPanel:**This is a UI-Panel with 2 child-objects that are UI-Buttons.

**RectTransform anchors** for the buttons should be set to:
ButtonOption1: Left Edge of parent (vertically aligned to center)
ButtonOption2: Right Edges of parent (vertically aligned to center)

**Attach CanvasGroup and Hide_Show_Panel.cs**
- Configuration for **Hide_Show_Panel.cs on DecisionPanel: **
- ShowOnStart not checked - default is false
- CloseButton: empty
- OpenButton: empty
**Once Configured: Create a prefab** so this can be reused in other scenes.

