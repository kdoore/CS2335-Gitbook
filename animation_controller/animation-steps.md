# 2D Animation Steps

To create a 2D animated character, first you need to find a set of sprites that show a character with slight changes in body position.

[Link to Free - 2D GameArt - Sprites used here](http://www.gameart2d.com/freebies.html)

After downloading the sprite set, determine which animation states your character will use.  In this case, we'll implement 4 animation states:  `idle, walk, jump, dead`  (_If desired, you can implement 3 states: idle, walk, jump_) 

  - **Import sprites **associated with desired animation states into your Unity project.  Put these in a folder: Sprites/PlayerSprites

![](/assets/Screen Shot 2018-10-31 at 10.42.11 AM.png)

- Select \( shift - to select multiple sprites \) 
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

- **Repeat steps** for each desired Animation State \( idle, walk, jump, dead \)

**Delete the following:**

  - Delete auto-created GameObjects: all **except** the **idle gameObject** \( the animation auto-created gameObjects\)
  - Rename the remaining gameObject: player
  - Add Tag:  player

- Find the Animator Component on the player gameObject, Identify the Animator Controller **\(idle\) **that is on the player gameObject.  
  **This one will not be deleted \(idle\)**.  
 
 ![](/assets/Screen Shot 2018-10-31 at 11.12.37 AM.png)

- Delete all other Animator Controller \(dead, walk, jump \)
- Keep all auto-created Animation clips (Don't delete these)

- The image below shows the remaining assets: 4 animation clips and 1 animator controller \( idle \) 
 ![](/assets/Screen Shot 2018-10-31 at 11.15.01 AM.png)

**Modify ** the auto-created ** idle Animator Controller: Create 3 new Empty States**

- Open the Animator Window - \( menu: window/Animator\)  
- With the player gameObject selected, the animator window should display as below.
  
  ![](/assets/Screen Shot 2018-10-31 at 11.23.46 AM.png)
  
- Right-click in the Animator window to **Create State > Empty**  
  
  ![](/assets/Screen Shot 2018-10-31 at 11.26.04 AM.png)

  Repeat 2 times to create  3 new State nodes  
  Rename States: right-click each state to rename: hero\_walk, hero\_jump, hero\_dead

  ![](/assets/Screen Shot 2018-10-31 at 11.30.03 AM.png)

**Configure new States**

- Configure state-nodes as shown in tables below.  
  - Each state-node must be assigned an animation clip, 
  - Select the state-node in the animator window, then set _**Motion**_ in the inspector by selecting the corresponding animation clip listed below.
  - Each state-node must have event arrows created and configured to allow transitions between state-nodes
  - Right-click on a state, select: Make Transition, drag to next state node according to the tables below 
  
  ![](/assets/Screen Shot 2018-11-08 at 9.00.12 AM.png)

###Create Animator Parameter:  HeroState
In the image below, in the left hand panel of the Animator Controller, select the **Parameter** tab.  Select **+** to add a new int parameter, you will name it **HeroState**.  This is the value that is used to set the logic for transition arrows between states.  The is is used in the code to send a signal to the animator controller in the player controller script.
![](/assets/Screen Shot 2018-12-01 at 7.19.31 AM.png)

![](/assets/Screen Shot 2018-12-01 at 7.15.35 AM.png)

```
//Code in PlayerController to set the HeroState parameter in the animator controller.
 animator.SetInteger("HeroState", (int)heroState.idle);
```



State Configuration:
 
| State |  Motion - Animation Clip | Create Transition Arrows |
|-----------|-------|-----------|
| hero_idle | _hero_idle animation_ | hero_idle -> hero_walk |
| hero_idle | _hero_idle animation_ | hero_idle -> hero_jump |
| hero_walk | _hero_walk animation_ | hero_walk -> hero_idle |
| hero_walk | _hero_walk animation_ | hero_walk -> hero_jump |
| hero_jump | _hero_jump animation_ | hero_jump -> hero_idle |
| hero-dead | _hero_dead animation_ | Any State -> hero_dead |



###State-Event Transition Table
The following table shows the details for logic that is used to configure the transition arrows between states
Each line of the table represents logic for an arrow between the CurState and NextState nodes. 


**State - Event Transition Table**

| Current State | Event Condition | Next State | HasExitTime |
|-----------|-------|-----------|-------------|
| hero_idle | HeroState == 1 | hero_walk | False |
| hero_idle | HeroState == 2  | hero_jump |  False|
| hero_walk | HeroState == 0 | hero_idle | False |
| hero_walk | HeroState == 2 | hero_jump | False |
| hero_jump | HeroState == 0 | hero_idle | True |
| Any State | HeroState == 3 | hero_dead | False |

The diagram below shows that 6 transition arrows have been created in the Animator Controller, using the configuration information listed in the table above.

![](/assets/Screen Shot 2018-11-08 at 8.55.16 AM.png)

###Configure Loop-time for the hero_dead animation clip

Since we don't want the hero_dead animation clip to loop continuously, we need to set that configuration as part of the hero-dead animation clip asset. In the project assets panel, find and select the hero-dead animation clip.  Once the animation-clip is selected, in the inspector panel, uncheck the Loop Time checkbox.  Loop-time will remain checked for all other animation clips.

**Select hero_dead Animation Clip in Project Assets **
![](/assets/Screen Shot 2018-11-08 at 9.23.39 AM.png)

**Uncheck Loop Time in Inspector Panel for hero-dead Animation clip**
![](/assets/Screen Shot 2018-11-08 at 9.23.55 AM.png)

###Add Animation Event to hero_dead Animation Clip
Finally, you may choose to add an animation-event to the hero_dead Animation clip.  An animation event allows for a function/method to be executed when at a specific animation keyframe is played.  In this case, we'd like the hero_dead animation to play completely before leaving the scene or reloading the scene. So, an animation event allows for some  specially defined event to be triggered when a specific keyframe is executed.  The animation event can be configured to execute any public method with the following syntax:  `public void someMethod( )`, but this method must be defined within a script-component that's on the same gameObject as the animation clip that has the animation event defined.

###Steps to Create An Animation Event 
  - Select the player in the hierarchy.
  - Open the Animation panel 
  - From the Animation panel dropdown, select the hero_dead animation clip
  - Select the timeline section above the final keyframe
  - Push the button: with icon: small white vertical rectangle, when you hover over this icon, it will say Animation Event  ![](/assets/Screen Shot 2018-11-08 at 11.32.38 AM.png)
  - In the inspector, select the Function from the dropdown, to be executed as the Animation Event.(See images below)
  - If the ReloadScene( ) method hasn't been added to the player controller yet, see code below, it will need to be customized for your game details.
  - You may want to add additonal keyframes, at a later keyframe time slice, if you want the dead animation to be displayed for a longer time before the scene is reloaded.
  
![](/assets/Screen Shot 2018-11-08 at 11.21.00 AM.png)

As seen in the image below, once you've configured an animation-event, then when you hovering over the icon, it will display the message shown below, which is the name of the function / method you've configured to be executed. 
 
![](/assets/Screen Shot 2018-11-08 at 11.22.20 AM.png)




```java
///Example Method in PlayerController.cs
///This method is executed from within the hero_dead animation, 
//when the keyframe is played with the corresponding animation event.

    public void ReloadScene(){
        if(GameData.instanceRef.Lives <= 0){
            //go to end scene if there are no more Lives left
            SceneManager.LoadScene("Scene5");  //actual scene name
            StateManager.instanceRef.SwitchState(new Scene5State());  //create new state, pass to StateManager

        }
        else //if there are still Lives left, reload the current scene 
        {
            //reload this current scene
            SceneManager.LoadScene("Scene4");  //actual scene name
            StateManager.instanceRef.SwitchState(new Scene4State());  //create new state, pass to StateManager
            //How should we reset score, health variables for the game?
        }

    }
```

