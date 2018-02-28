#Adventure Game - Part 1

For Project 2, we'll begin to create a framework for a branching-narrative adventure game.

###Starter Code - Assets: 
[Link to Assets zip file](https://utdallas.box.com/v/Project2-assets-S18)

[Link to Full Project zip file](https://utdallas.box.com/v/Project2-S18-StarterProject)

###2 Options
To get started on Project 2, you have 2 options, using the zip files above. You can either use Full Project starter-code , or follow steps below to import contents of the Assets folder into a new project's Asset folder.

###Option 2: Create New Project: Unity 2D 
 
After creating a new Unity 2D project, unzip the starter-code zip file from the link above, then drag each sub-folder into your project's assets panel.  

If you're not able to drag your sub-folders directly into the Project > Assets panel in Unity, then you can do the same process using your computer's file system browser.  To do this, first find your new project's asset folder by going to the Unity top menu: Assets > ShowInExplorer or RevealInFinder. Then, drag the sub-folders directly into the Assets folder in the newly opened Assets folder.



###Option 2: Add Scenes in Build Settings
For this project to work, you must first go into your project's Build Settings. (File > Build Settings)  You must add both scenes to the Scenes to Build Panel.  You can drag the scenes from your assets directly into the Scenes to Build Panel.  Note the ordering, BeginScene must be higher than EndScene in the order, it will have a 0 on the right side of the panel, while EndScene will have a 1.

![](/assets/Unity 2.JPG)

![](/assets/Screen Shot 2018-02-12 at 2.40.58 PM.png)

###Play the Game
If you've done the above steps correctly, you should be able to use the EndButton to go to the EndScene, and from there, you should be able to use the StartButton to return to the StartScene.

###Problems with TitlePanel and TitleText Display
Sometimes Unity acts glitchy with UI-Panels and UI-Text gameObjects.  If the TitlePanel and TitleText aren't displaying correctly, select the TitlePanel in the hierarchy and then select the rect-transform tool which shows the positioning blue circles at the corners of the panel.  Try dragging the top blue circles below the bottom circles to see if the panel has flipped backwards.  Also try dragging the left corners over the right corners.  This may also be necessary to do with the text element.  If these efforts don't fix the problem, simply delete the panel and add new panel / text gameObjects to recreate the corrupted gameObjects.
