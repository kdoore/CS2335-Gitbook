#PlayerController:  Version 1, Fall 2019

###Create a Prefab Player GameObject

1. Create Animations as described in prior section.  **Delete all except one **of the auto-created animatedSprite gameObjects in the hierarchy panel.
2.  Rename the Animated 2DSprite GameObject: **Player**
3.  Create a Folder in Assets:  **Scripts**
4. Create a C# Script, by 'right-clicking' to open the menu, from the Project/Assets/Scripts folder: **PlayerController**
5. Add the script, as a component, in the Inspector Panel of the Player gameObject.
6. Double-click the PlayerController.cs script in the Scripts folder, this script should open in Visual Studio
7. Paste code listed below into the PlayerController  class file in Visual Studio. 
8. Build Solution in Visual Studio to compile your script.
9. Create a Folder in Assets: **PreFabs**
10. Drag your Player GameObject from the Hierarchy Panel to the PreFab's folder in your assets panel.
11. Select the Player GameObject in the Hierarchy, in the Inspector Panel, it should be similar to the image below. 

![](/assets/Screen Shot 2019-09-09 at 1.56.40 PM.png)
###PlayerController.cs Script - Sept 9, 2019


```java

using System.Collections;
using System.Collections.Generic;
using UnityEngine;

public class PlayerController : MonoBehaviour
{
    //create custom data-type
    public enum HeroState { idle = 0, walk = 1, jump = 2}
    public HeroState currentHeroState;  //variable to display current HeroState in inspector

    private bool facingRight = true; // initialize when declare (first way),  false by default

    private Animator animator; //obj-reference variable to access the animator component on this gameObject

    //inspector is the second way to initialize a value

    // Start is called before the first frame update
    private void Start()
    {
        facingRight = true; //final way to initialize, overrides all prior settings
        currentHeroState = HeroState.idle;
        animator = GetComponent<Animator>(); //<T>  //call a method - make connection to component on gameObject in Unity scene
        animator.SetInteger( "HeroState",  (int) HeroState.idle      );
    }

    //Fixed update used for physics, to give smooth motion, called at consistent time increments
    void FixedUpdate()
    {
        float inputX = Input.GetAxis("Horizontal"); // key input:  -1, 0, 1
        bool isWalking = Mathf.Abs(inputX) > 0;

        bool jumpPressed = Input.GetButtonDown("Jump");

        if (isWalking)
        {
            currentHeroState = HeroState.walk;
            animator.SetInteger("HeroState", (int)HeroState.walk);
        }
        else
        {
            currentHeroState = HeroState.idle;
            animator.SetInteger("HeroState", (int)HeroState.idle);
        }


        if (jumpPressed)
        {
            currentHeroState = HeroState.jump;
            animator.SetInteger("HeroState", (int)HeroState.jump);
        }

    }

    private void Flip()
    {
        facingRight = !facingRight; //change the polarity of the value

        Vector3 theScale = transform.localScale;
        theScale.x *= -1; //change the x component, multiply by -1
        transform.localScale = theScale;


    }

}  //end class PlayerController

```

###PlayerController version1 Summary:  

**Goal:** Configure device input events to correspond with Animator Component 
		- Input Manager: horizontal-axis, key, 
		- enums, logic, 
		- goal: modify Animator HeroState parameter value
		- generate valid state-transition events


- **Animator Controller:** Integer Parameter: HeroState
	- Edit in Animator Window, with Player GameObject selected in Hierarchy
	
	- **Animator states**:  state-node 
		 - each state-node corresponds to Animation Clip (motion)
	
	- **Event transitions**:  transition arrows: 
		- each  transition arrow corresponds to a valid state transition-event
		- HeroState Conditions:
		- HeroState == 0:  idle state-transition
		- HeroState == 1:  walk state-transition
		- HeroState == 2:  jump state-transition

- **Animation Clip** - additional configuration using Animation Widnow


**Flip( ) **- modify left/right sprite orientation by modifying Transform.Scale.X of Player sprite.

**Next Versions of Player Controller to add: **

**Rigidbody** -  physics motion ex. gravity

Collider2D

Floor - empty gameObject with 

Sorting Layers - determine render ordering of sprites, etc

Physics Layers:  Determine physics interactions: 
- example: Custom Layer: Ground
