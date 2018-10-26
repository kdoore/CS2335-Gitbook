#DialogManager for Scriptable Objects
Updated Oct 26, 2018

###NextPanel To Open
The code below has logic to open the next panel, when the dialog is complete.  If there is no nextPanelToOpen configured in the inspector, then no error will occur because the code first checks to see if that variable contains a valid object (memory address)  

   
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

    void GetNextDialog()
    {
        bool moreDialog = NextDialog();
        if (!moreDialog)
        {
            Utility.HideCG(dialogPanelCG); // close panel
            CloseDialog();
            Utility.ShowCG(nextPanelToOpenCG);
        }
    }
	
    //this method is called if there is an openDialog button set in the Inspector
    //called when this button is clicked
    public void OpenDialog()
    {
        Utility.ShowCG(dialogPanelCG);
        NextDialog();
        openDialogBtn.gameObject.SetActive(false);
    }

    public void CloseDialog()
    {
        Utility.HideCG(dialogPanelCG);
        Debug.Log("Closing Dialog");
        //check if there is a panel (CG) to diplay when dialog is done
        if(nextPanelToOpenCG != null){
            Utility.ShowCG(nextPanelToOpenCG);
        }
    }

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

