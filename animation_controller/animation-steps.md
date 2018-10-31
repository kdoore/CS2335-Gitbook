#2D Animation Steps

To create a 2D animated character, first you need to find a set of sprites that show a character with slight changes in body position. 

[Link to Free - 2D GameArt - Sprites used here](http://www.gameart2d.com/freebies.html)

After downloading the sprite set, determine which animation states your character will use.  In this case, we'll implement 4 animation states:  `idle, walk, jump, dead`

    - **Import sprites **associated with desired animation states into your Unity project.  Put these in a folder: Sprites/PlayerSprites

![](/assets/Screen Shot 2018-10-31 at 10.42.11 AM.png)

    - Select ( shift - to select multiple sprites ) 
    - Drag the set of sprites into the Scene view, you will be prompted to SaveAs,  'Create a new animation' 
    
 ![](/assets/Screen Shot 2018-10-31 at 10.41.50 AM.png)
    
    This creates the following Items:
    - A gameObject in the hierarchy with an attached: **Animator Component**
    
    - An **Animation clip** - in the selected assets folder. This can be viewed/edited in the Animation window when the gameObject is selected
       
         * Animation clip:   ![](/assets/Screen Shot 2018-03-22 at 12.26.09 PM.png)


        - An **Animator Controller** - in the selected assets folder. This can be viewed/edited in the Animator window when the gameObject is selected
        
            - Animator Controller  ![](/assets/Screen Shot 2018-03-22 at 12.27.35 PM.png)


