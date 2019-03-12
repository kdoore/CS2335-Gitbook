#Configure TitlePanel, DecisionPanel

##Configure Other Items Used for Simple Dialog

###Hide_Show_Panel, CanvasGroup
![](/assets/Screen Shot 2019-03-06 at 3.25.54 PM.png)


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




