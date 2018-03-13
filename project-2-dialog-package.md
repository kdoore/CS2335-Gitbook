#Project 2 - Dialog Package

The link below is to a Unity package file that can be imported as an asset into your project so you can use it to create some simple dialog for your project.

[Link to Unity Package:](https://utdallas.box.com/v/Dialog-Package)

###Import package
To install the package, I suggest that you first import this package into a new 2D Unity project, to see if you want to use it in your project. Otherwise, I'd strongly recommend saving a backup of your project before importing the Unity package.  To import into a Unity project, go to Assets > Import Package > Custom Package, then browse to the downloaded package file from the link above.  Select all assets for import.

###Panel Prefabs - Add one as a child of the Canvas
This project contains 2 main prefab objects in the Resources folder.  Both prefabs contain a top-level UI-Panel that acts as a container for the child UI-gameObjects.  Before either of the Panel prefabs can be added to a scene, there must be a UI-Canvas in the scene, and **the prefab must be added as a child of the Canvas**, this can be done by dragging the prefab from the assets panel and dropping the prefab directly on top of the Canvas gameObject in the Hierarchy panel.

Once the prefab has been added to a scene, then find the script component: 

###Creating your own Scriptable Object

###Configuring your Conversation
The image below shows that when you create a new conversation, you need to set the colors for the text for both speakers.  **IMPORTANT TO NOTE - text and images will be invisible if you don't configure these values, they must be changed from the default values for a new conversation** - These colors have a default value when first created - of full transparancy, this must be changed, even if you just want to use black text.  The SpeakerFadeAlpha and SpeakerFullAlpha control the image alpha, these also default to an alpha of 0.  Compare to the example conversations to determine how you'd like to set these values.
![](/assets/Screen Shot 2018-03-06 at 12.41.25 PM.png)

