#DialogManager for ConversationList

Also see [Hide_Show_Panel Script ](/conversation-scriptable-objects/dialogmanagerconvlist/hideshow-panel-script.md)

###Inspector Images

The images below show the Inspector panel configuration for the DialogPanel and Decision panels.  **Note that the DialogPanel has had the Image(Script) component removed.**  If the Image(Script) isn't removed from the DialogPanel, then the character images will show up in the panel background instead of in the CharacterImage gameObject, this is because we are displaying the CharacterImage in the first child of the DecisionPanel, and it finds it's own Panel image as the first child if that image component isn't removed.

![](/assets/Screen Shot 2018-10-27 at 5.57.09 AM.png)
![](/assets/Screen Shot 2018-10-27 at 6.26.21 AM.png)
![](/assets/Screen Shot 2018-10-27 at 6.24.47 AM.png)


###NextPanelToOpen
The DialogManager script has a public reference variable that can be set in the Inspector allows for the nextPanelToOpen to be populated. After all of the dialog entries has been displayed, the dialogPanel is closed, and then the nextPanelToOpen will be opened.  If not  populated, no error will occur because the code first checks to see if that variable contains a valid object reference (memory address). 

Add the linked script to the DecisionPanel so it will stay closed at the start of the scene, if desired [Hide_Show_Panel Script ](/conversation-scriptable-objects/dialogmanagerconvlist/hideshow-panel-script.md)


 ###OpenDialogBtn
 The OpenDialogBtn public reference in the Inspector should be populated if a button will be used to open the dialog.  Otherwise, the ShowOnStart checkbox in the Inspector should be selected, it will make the panel visible at the beginning of the scene..  

   
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

    public ConversationList convList; //attach scriptable object in Inspector
    public CanvasGroup nextPanelToOpenCG;  //next panel to open, set in Inspector
    public Button openDialogBtn; //Button that will open Dialog
    public bool showOnStart = false;

    private Button nextDialogBtn;
    private CanvasGroup dialogPanelCG;
    private Text dialogText, speakerName; //speakerName;
    private Image speakerImage;
    private int conversationIndex;

    // Use this for initialization
    void Start () {

        conversationIndex = 0;

        dialogPanelCG = GetComponent<CanvasGroup>(); //used to show/hide panel
       
        Text[] childTextElements = GetComponentsInChildren<Text>();

        speakerName = childTextElements[0]; //first child of Panel
        speakerImage = GetComponentInChildren<Image>();

        if (openDialogBtn != null ) //if opening dialog with a Button, Populate OpenDialogButton in the Inspector 
        {
            openDialogBtn.onClick.AddListener(OpenDialog);
        }

        nextDialogBtn = GetComponentInChildren<Button>();
        nextDialogBtn.onClick.AddListener(GetNextDialog);

        dialogText = childTextElements[1]; //2nd child text element

        //checkbox that can be set in inspector, if checked, then this is not exected
        if (!showOnStart)
        {
            Utility.HideCG(dialogPanelCG);
        }
        else{ //if showing on scene load, get first Dialog 
            Utility.ShowCG(dialogPanelCG);
            NextDialog();
        }
    }

    /// <summary>
    /// Gets the next dialog.
    /// If no more dialog, close the dialog panel
    /// open the nextPanelToOpen
    /// </summary>
    void GetNextDialog()
    {
        bool moreDialog = NextDialog();
        if (!moreDialog)
        {
            Utility.HideCG(dialogPanelCG); // close panel
            CloseDialog();
            if (nextPanelToOpenCG != null) //check to see if valid gameObject was set in inspector
            {
                Utility.ShowCG(nextPanelToOpenCG);
            }
        }
    }


    /// <summary>
    /// Opens the dialog.
    /// this method is called if there is an 
    /// openDialog button set in the Inspector
    /// otherwise, select checkbox showOnStart 
    /// </summary>
    public void OpenDialog()
    {
        Utility.ShowCG(dialogPanelCG);
        NextDialog();
        openDialogBtn.gameObject.SetActive(false);
    }

    /// <summary>
    /// Closes the dialog.
    /// </summary>
    public void CloseDialog()
    {
        Utility.HideCG(dialogPanelCG);
        Debug.Log("Closing Dialog");
        //check if there is a panel (CG) to diplay when dialog is done
        if(nextPanelToOpenCG != null){
            Utility.ShowCG(nextPanelToOpenCG);
        }
    }

    /// <summary>
    /// Nexts the dialog.
    /// Sets UI elements: speakerName, speakerImage, dialogText
    /// </summary>
    /// <returns><c>true</c>, if dialog there is more dialog, <c>false</c> otherwise.</returns>
    public bool NextDialog()
    {   
        if (conversationIndex < convList.Conversation.Count)
        {   speakerName.text = convList.Conversation[conversationIndex].speakerName;
            speakerImage.sprite = convList.Conversation[conversationIndex].speakerImg;
            StopAllCoroutines();
            string curSentence = convList.Conversation[conversationIndex].dialogTxt;
            StartCoroutine(TypeSentence(curSentence));
        }
        else
        {
            conversationIndex = 0;
           return false;  //if no more list elements return false
        }

        conversationIndex++; //increment index for next conversation item
        return true;
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
}

```

