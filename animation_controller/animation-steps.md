#2D Animation Steps

To create a 2D animated character, first you need to find a set of sprites that show a character with slight changes in body position. 

[Link to Free - 2D GameArt - Sprites used here](http://www.gameart2d.com/freebies.html)

After downloading the sprite set, determine which animation states your character will use.  In this case, we'll implement 4 animation states:  `idle, walk, jump, dead`

    - **Import sprites **associated with desired animation states into your Unity project.  Put these in a folder: Sprites/PlayerSprites

![](/assets/Screen Shot 2018-10-31 at 10.42.11 AM.png)

    - Select ( shift - to select multiple sprites ) 
    - Drag the set of sprites into the Scene view, you will be prompted to SaveAs,  'Create a new animation' 
    
 ![](/assets/Screen Shot 2018-10-31 at 10.41.50 AM.png)
    
  **  This creates the following Items:**
    - A gameObject in the hierarchy with an attached: **Animator Component**
        - Animator Component:  ![](/assets/Screen Shot 2018-10-31 at 11.08.46 AM.png)
    
    - An **Animation clip** - in the selected assets folder. This can be viewed/edited in the Animation window when the gameObject is selected
       
         - Animation clip:   ![](/assets/Screen Shot 2018-10-31 at 11.11.21 AM.png)


        - An **Animator Controller** - in the selected assets folder. This can be viewed/edited in the Animator window when the gameObject is selected
        
            - Animator Controller  ![](/assets/Screen Shot 2018-10-31 at 11.11.12 AM.png)

- **Play the Unity scene** to make sure the created gameObject animation works as expected

- **Repeat steps** for each desired Animation State ( idle, walk, jump, dead )

**Delete the following:**
    - Delete all except the **idle gameObject** ( the animation auto-created gameObjects)
    - Rename the remaining gameObject: player
    - Add Tag:  player
    
    - Find the Animator Component on the player gameObject, Identify the Animator Controller **(idle) **that is on the player gameObject.
     **This one will not be deleted (idle)**.   ![](/assets/Screen Shot 2018-10-31 at 11.12.37 AM.png)
    - Delete all other Animator Controller (dead, walk, jump )
    - The image below shows the remaining assets: 4 animation clips and 1 animator controller ( idle ) 
    - ![](/assets/Screen Shot 2018-10-31 at 11.15.01 AM.png)
    
**Modify ** the auto-created ** idle Animator Controller:**
    - Open the Animator Window - ( menu: window/Animator)  
    - With the player gameObject selected, the animator window should display as below.
    ![](/assets/Screen Shot 2018-10-31 at 11.23.46 AM.png)
