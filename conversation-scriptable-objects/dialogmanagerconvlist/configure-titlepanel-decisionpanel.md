#Configure TitlePanel, DecisionPanel

Configure other GameObjects used with Dialog Prefabs

For any UI-Panel that you'd like to show/hide, you must add a CanvasGroup, and the Hide_Show_Panel script as shown in the image below.

###Hide_Show_Panel, CanvasGroup
![](/assets/Screen Shot 2019-03-06 at 3.25.54 PM.png)


###TitlePanel - Prefab
The image below shows the hierarchy panel for a TitlePanel that has a UI text, and a UI-Button that can be used to open a dialog prefab.

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
This prefab is a UI panel that contains 2 buttons that can be used for scene change logic in all scenes.  It should start as hidden, then it's opened by the DialogPanel's script, when there's no dialog left to display.  It is configured as the "NextPanelToOpen" for the SimpleDialog and DialogManager scripts.

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




