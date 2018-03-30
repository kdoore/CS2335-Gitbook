# Canvas: Screen-Space Mode

For our 2-D Projects, we'll want to include several different types of images, some images will be sprites that we'll incorporate into game-play logic such as NPC characters, Player character etc. These will be added to our project as: GameObject -&gt; 2D -&gt; Sprite.  We'll include some other images as part of the user-interface \(UI-Images\).  These will be added to our project as UI -&gt; Image.  These Images are associated with the Canvas, and by default, they are child objects of the Canvas.

If we want to have size consistency for sizing these 2 different types of images between scenes, we can implement a single rendering mode for both Canvas objects and other types of Game Objects across all scenes.  To do that, we'll implement the convention that all Canvas gameObjects use: Canvas RenderMode: Screen Space - Camera, with the Main Camera selected as the Render Camera.  This configuration can be setup for a scene once a UI object has been added to the scene, so that the Hierarchy Panel has a Canvas gameObject.  Next, select the Canvas GameObject in the Hierarchy and and modify the Canvas Component in the Inspector Panel. \(See image below\).

###Screen Space Camera
Select the 'Screen Space - Camera' item from the Render Mode drop-down list.  Once that's been selected, then a new option shows up: Render Camera.  Select the black circle to the far right of the Render Camera variable-box, and it will pop-up a contextual selector where you can choose the Main Camera to populate this box.  You can also drag the Main Camera from the Hierarchy Panel to populate this box.  Once this step has been completed, make sure to save the current Scene, to save this configuration.  This setting must be configured individually for each Scene.



![](Screenshot 2016-02-15 20.14.52.png)

