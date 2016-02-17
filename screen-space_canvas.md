# Screen-Space Canvas

For our Text-Adventure Project, we'll want to include several different types of images, some images will be sprites that we'll incorporate into game-play logic such as NPC characters, Player character etc. These will be added to our project as: GameObject -> 2D -> Sprite.  We'll include some other images as part of the user-interface (UI-Images).  These will be added to our project as UI -> Image.  These Images are associated with the Canvas, and by default, they are child objects of the Canvas. 

If we want to have size consistency for sizing these 2 different types of images between scenes, we can implement a single rendering mode across all scenes.  To do that, we'll implement the convention that all Canvas gameObjects use: Canvas RenderMode: Screen Space - Camera, with the Main Camera selected as the Render Camera.  

![](Screenshot 2016-02-15 20.14.52.png)




