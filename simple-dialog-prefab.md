#Simple Dialog Prefab

How can we create a simple dialog system in Unity?  
We should try to make something that will work as a prefab, so we can use it in any scene, as many times as we'd like. 
- We need to include
    - UI-panel as a container with a CanvasGroup component   so we can toggle it's visibiltiy
    - UI-Text, a child of the UI-panel, this will display the text
    - UI-Button, a child of the UI-panel, this will allow us to advance through the dialog items
    - A custom script component- attached to the UI-Panel
    will have logic for the dialog system.
      -Script must use a public List of strings that can be populated in the inspector, with new dialog each time the panel-prefab is used. 

 
Prefab Link Added March 12 Spring19      
[Link to UnityPackage with Prefab panels](https://utdallas.box.com/v/simple-dialog-prefab-S19)
You must add the SimpleDialog script to the Prefab, it is not included with this package.

 ###Prefab GameObject: Hierarchy     
 The image below shows the Hierarchy panel structure of these gameObjects.  The **SimpleDialog.cs** script is attached to the top-level panel object: **DialogPrefab**.  The panel must also have a **CanvasGroup **component attached. To add a CanvasGroup component, in the Inspector panel: Add Component>Layout > CanvasGroup

![](/assets/Screen Shot 2019-03-06 at 2.27.16 PM.png)
![](/assets/Screen Shot 2019-03-06 at 2.32.44 PM.png)

The DialogText is anchored to 4 corners of its' parent, the DialogPanel.  The NextButton is anchored to the bottom  corner of the panel (not shown here).

**Prefab:** You will create a Prefab of this after adding and configuring the SimpleDialog script component.

###Important - Order The Text Elements As Children 1,2,3:
    - Make sure that the **DialogText is the first child **of the DialogPanel, and that the NextButton is below the DialogText in the Hierarchy Panel, the **SpeakerName is the 3rd child** Text element of the main panel:  DialogPrefab.
    
    - In the code, we're finding the DialogText, and the SpeakerText components since it's the first (and third) children objects of the SimpleDialogPanel that has a Text component.  If the Button is the first child, then it's text is what'll show the dialog text.

#DialogPanel Logic
The image below shows part of the Inspector panel for the DialogPrefab.  It shows a CanvasGroup component and a SimpleDialog.cs script component have been added. The SimpleDialog script has a Conversations List with adjustable size, each element can hold some dialog text,speakerName, (and Sprite) which will be displayed sequentially when the scene is played.

![](/assets/Screen Shot 2019-03-06 at 2.34.53 PM.png)

#ConversationEntry.cs Custom Script

Below is the code for a simple class that has elements for each ConversationEntry: Since we use the Class Attribute: 
[System.Serializable], then we'll be able to display and populate each ConversationEntry in our list in the inspector panel.


```java

using UnityEngine;
using UnityEngine.UI;
using System.Collections;

[System.Serializable]
public class ConversationEntry
{
    public string speakerName;

    [TextArea]
    public string dialogTxt;

    public Sprite speakerImg;
}
```

#DataStructures: List<T>, Queue<T> - For ConversationEntries
Now, we just have to figure out how to write the code logic. We need an List of ConversationEntry items, then we need to display them sequentially when the next button has been clicked.

Unity can display for editing, both List< string >, or array: string[] in the inspector below. 

**List< T >** is part of the System.Collections.Generic Namespace in C#.  [MSDN Reference](https://msdn.microsoft.com/en-us/library/6sh2ey19.aspx)

**Queue< T >** is a data structure that operates like a queue / waiting line.  It will make it easy to remove each sequential dialog item from the collection so it can be displayed in sequence.[MSDN Reference](https://msdn.microsoft.com/en-us/library/7977ey2c.aspx)


#SimpleDialog.cs 
(See complete code at bottom of page)

- **Declare Object Reference Variables - ConversationEntry**
   
  -  In the code below we specify that the `List< T >` and `Queue< T >` will both be collections of `ConversationEntry` objects.

```java
    private Queue<ConversationEntry> conversationsQueue = new Queue<ConversationEntry>();
    public List<ConversationEntry> conversations; //initialized by Unity in Inspector

```
  - **Declare object reference variables for the components ** we'll interact with. 

```java
    public Button openButton;
    public CanvasGroup nextPanelToOpen;
    public bool showOnStart = false;

    //find in children
    private Button nextButton; //only 1 child button
    private Text dialogText;   //find as a child - Hierarchy order mattersprivate CanvasGroup dialogCG;  //Canvas Group on top level - script attached to this Panel
    private Text speakerText;  //find as a child - Hierarchy order matters
    private CanvasGroup dialogCG; //top level panel

```
- **Start: Initialize Object Reference Variables**
The following logic would be located in the Unity Start( ) event function for this script that's attached to the panel gameObject:

```java
 // Use this for initialization
    void Start()
    {
        dialogCG = GetComponent<CanvasGroup>();
    //Find all children Text elements of current Panel GameObject
        Text[] textChildren = GetComponentsInChildren<Text>();
        dialogText = textChildren[0]; //first child Text
        speakerText = textChildren[2]; //3rd child Text

        InitializeDialog(); //populate Queue, set initial conversation values

        nextButton = GetComponentInChildren<Button>();
        nextButton.onClick.AddListener(GetNextDialog);
        if (!showOnStart) //Should this be hidden at Start
        {   //is there a button to display panel
            if (openButton != null)
            {
            openButton.onClick.AddListener(ShowDialogPanel);
            }
            Utility.HideCG(dialogCG); //hide initially
        }
        else //show on start
        {
            Utility.ShowCG(dialogCG);
        }

    }//end start

```
-  **GetComponentsInChildren< Text >()**See section below for details     

- **Populate the queue data structure** 

###Initialize DialogQueue
In the code below we specify that the `List< T >` and `Queue< T >` will both be collections of `ConversationEntry` objects.

```java
void InitializeDialog()
    {
        foreach( ConversationEntry item in conversations)
        {
            conversationsQueue.Enqueue(item); //put each string -item in the queue
        }
        GetNextDialog();  //get first item
    }
```

###GetNextDialog Method
The nextButton allows the user to move forward through the dialog items.  In the Start( ) method, we configured the nextButton's onClick method to execute the GetNextDialog method each time it is clicked.  The code below shows that each time the GetNextDialog method is executed, it first checks to make sure there are items in the queue.  Then, the dialogText is updated to display the first item in the queue.  The Queue< T > Dequeue( ) method retrieves and removes the item at the front of the queue returns that value so it can be used to set the text value for the dialogText.  If there are no more items in the queue, then the panel is hidden, using the Utility class static method: HideCG.

```java
void GetNextDialog()
    {
        if (conversationsQueue.Count > 0)
        {
            ConversationEntry item = conversationsQueue.Dequeue();
            dialogText.text = item.dialogTxt;
            speakerText.text = item.speakerName;
        }
        else { //no more dialog
            if ( nextPanelToOpen != null)
            {
                Utility.ShowCG(nextPanelToOpen);
            }
            Utility.HideCG(dialogCG); //hide the dialog
        } 
    }
    
    //Helper Method, allows Button-click To Execute Utility.ShowCG( )
    public void ShowDialogPanel()
    {
        Utility.ShowCG(dialogCG);
    }//end function
```

###Unity: GetComponentsInChildren< T >()
The Unity method: GetComponentsInChildren, provides a convenient way to initialize an object reference variable based on the Hierarchy panel's parent-child relationships between gameObjects. 

In the code above, we've specified that we want to initialize the Text component reference variable: dialogText, speakerText. Since the current script is on the DialogPanel in our custom prefab, and since there is a UI-Text gameObject that is a child of the DialogPanel, we can access the components on that child object using this method.  

**Important:**  Note that this method will find all < T > components while traversing the Parent-child relationships of the gameObject that this script is attached to.  So, make sure to order your gameObjects in the hierarchy correctly if you plan to use this method.

Unity also has a similar method: GetComponentsInChildren< T >(),  which returns an array of all matching object in children that are ordered according to parent-to children ordering in the hierarchy.[Unity Manual](https://docs.unity3d.com/ScriptReference/Component.GetComponentInChildren.html)
        

#SimpleDialog.cs Complete Code
```java

using System.Collections;
using System.Collections.Generic;
using UnityEngine;
using UnityEngine.UI;

public class SimpleDialog : MonoBehaviour
{

    public Button openButton;
    public CanvasGroup nextPanelToOpen;
    public bool showOnStart = false;

    //find in children
    private Button nextButton; //only 1 child button
    private Text dialogText;   //find as a child - Hierarchy order mattersprivate CanvasGroup dialogCG;  //Canvas Group on top level - script attached to this Panel
    private Text speakerText;  //find as a child - Hierarchy order matters
    private CanvasGroup dialogCG; //top level panel

    private Queue<ConversationEntry> conversationsQueue = new Queue<ConversationEntry>();
    public List<ConversationEntry> conversations;

    // Use this for initialization
    void Start()
    {
        dialogCG = GetComponent<CanvasGroup>();
        Text[] textChildren = GetComponentsInChildren<Text>();
        dialogText = textChildren[0];
        speakerText = textChildren[2];

        InitializeDialog();

        nextButton = GetComponentInChildren<Button>();
        nextButton.onClick.AddListener(GetNextDialog);
        if (!showOnStart)
        {   //click button to display panel
            if (openButton != null)
            {
                openButton.onClick.AddListener(ShowDialogPanel);
            }
            Utility.HideCG(dialogCG); //hide initially
        }
        else //show on start
        {
            Utility.ShowCG(dialogCG);
        }

    }//end start

    void InitializeDialog()
    {
        foreach( ConversationEntry item in conversations)
        {
            conversationsQueue.Enqueue(item); //put each string -item in the queue
        }
        GetNextDialog();  //get first item
    }

    void GetNextDialog()
    {
        if (conversationsQueue.Count > 0)
        {
            ConversationEntry item = conversationsQueue.Dequeue();
            dialogText.text = item.dialogTxt;
            speakerText.text = item.speakerName;
        }
        else { //no more dialog
            if ( nextPanelToOpen != null)
            {
                Utility.ShowCG(nextPanelToOpen);
            }
            Utility.HideCG(dialogCG); //hide the dialog
        } 
    }

    public void ShowDialogPanel()
    {
        Utility.ShowCG(dialogCG);
    }//end function

} //end class

```

##Configure Other Items Used for Simple Dialog

###Hide_Show_Panel, CanvasGroup
![](/assets/Screen Shot 2019-03-06 at 3.25.54 PM.png)


###TitlePanel - Prefab
![](/assets/Screen Shot 2019-03-06 at 2.26.55 PM.png)


**TitlePanel:**  This is a UI-Panel with children: 
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




