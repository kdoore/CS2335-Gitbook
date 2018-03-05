#Project 2 - Dialog Package

The link below is to a Unity package file that can be imported as an asset into your project so you can use it to create some simple dialog for your project.

[Link to Unity Package:](https://utdallas.box.com/v/Dialog-Package)

###Import package
To install the package, you can try the dialog on a new 2D project to see if you want to use it in your project, otherwise, I'd strongly recommend saving a backup of your project before importing.  To import into a Unity project, go to Assets > Import Package > Custom Package, then browse to the downloaded package file from the link above.  Select all assets for import.

###Panel Prefabs - Add one as a child of the Canvas
This project contains 2 main prefab objects in the Resources folder.  Both prefabs contain a top-level UI-Panel that acts as a container for the child UI-gameObjects.  Before either of the Panel prefabs can be added to a scene, there must be a UI-Canvas in the scene, and **the prefab must be added as a child of the Canvas**, this can be done by dragging the prefab from the assets panel and dropping the prefab directly on top of the Canvas gameObject in the Hierarchy panel.

Once the prefab has been added to a scene, then find the script component: 

