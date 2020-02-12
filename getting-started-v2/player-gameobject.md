#Player GameObject

Follow Steps below to create the and configure the Player GameObject

####Create a 2D Sprite GameObject - Player:

Select **GameObject** in the top menu, select: **GameObject &gt; 2D Object &gt; Sprite,** this should add a new item in the **Hierarchy panel**.  Select this new gameObject in the Hierarchy Panel,  Now, the **Inspector panel** should show details of this selected gameObject, **rename** the gameObject by typing the 'Player' or  into the top textbox in the Inspector panel. To **select a sprite** image \(from the assets tab\) to associate with this new 2D object, you will select/press the small circle icon to the right of the Sprite Renderer Component's **Sprite** field. This will create a new pop-up window that shows all possible items in the assets that can be selected as the sprite to be rendered.  

![](/assets/SelectSpriteImg.png)

###Physics2D
The Physics2D Engine manages movement and collisions for 2D gameObjects.  We'll add Rigidbody2D for gameObjects that need to move in a scene.  We'll add Collider2D components to give gameObjects a physical boundary for collisions with other gameObjects.

![](/assets/Screen Shot 2019-01-17 at 2.47.49 PM.png)

####RigidBody2D
This component puts a gameObject's movement under control of the Physics2D engine.  The gameObject's movement should be managed using Physics2D methods which can add forces to the object, change the velocity, etc.  The Physics2D engine should be used to determine a moving gameObject's changing position values. In contrast, moving a gameObject by changing the transform position values is not recommended.  The Rigidbody2D component manages movement of the gameObject's colliders in an optimized manner. You will need to expand the Constraints section so you can FreezeRotation for the Z axis as shown in the image above.
**Any gameObject that will be moving in a scene should have a Rigidbody2D component. **

####Collider2D Components
This component gives sprites a physical presence, it gives them a collision boundary, it allows for collision interactions with other gameObjects that also have Collider2D component.  GameObjects that will have any movement during gameplay should have an attached Rigidbody2D component which also manages movement of the object's Collider2D's.  Multiple colliders can be used on a single gameObject, if the boundaries overlap, they will act as a composite collider for the gameObject.  Sometimes you may also have a small collider nested within a larger collider, where the outer-collider can function as a trigger, but the inner collider will interact with the floor's collider to keep the object within the scene.

* **IsTrigger** - when checking this checkbox: then this collider **will not display collision interaction behavior**, but it will cause the **OnTriggerEnter2D** event to be exectued. This is often used for sensing movement into zones, or for for objects that will be destroyed.   

###Steps to Create and Configure 2D Sprite GameObjects:

1. **Background** Create a 2D Sprite GameObject by selecting a sprite that can be used as a background image - scale to fill the camera's viewport. **Objects higher in the Hierarchy panel are rendered behind gameObjects lower in the Hierarchy.** 

Use **Sorting Layers** as the preferred method for ordering sprite layering for rendering. [Unity Tutorial Video on SortingLayers](https://unity3d.com/learn/tutorials/topics/2d-game-creation/sorting-layers)

2. **Player** Create a 2D Sprite GameObject
    * Select desired sprite from the Assets for the SpriteRenderer component's sprite field (as detailed above).
    * Add **Physics2D &gt; RigidBody2D** Component - this is required for objects that will have **movement**, physics forces should be used to give movement to gameObjects.
    
    * Add **Physics2D &gt; Collider2D** Components - select 1 or more Colliders to fit your gameObject.  Select the Edit-collider button to change the size of the collider, manually change the x or y offset.
   
   - Create C\# Script:  **PlayerController** - Simple Version \(script provided below
   
  - Add PlayerController as a Script Component to Player GameObject  
  
  
  #PlayerController Code: Sept 16
  


```java

using System.Collections;
using System.Collections.Generic;
using UnityEngine;

public class PlayerController : MonoBehaviour
{
    //create custom data-type
    public enum HeroState { idle=0, walk=1 , jump=2 }

    public HeroState currentHeroState;  //variable to display current HeroState in inspector

    private bool facingRight; // initialize when declare (first way),  false by default

    private Animator animator; //obj-reference variable to access the animator component on this gameObject
    private Rigidbody2D rb2D; //variable to refer to the rigidbody component on this GObj
    public float forceX = 50f;  ///probably too small
    public float jumpForce = 8.0f; ///probably needs changed
    public bool grounded; //is the player currently touching a GObj w/ layer Ground
    private bool jump; //has jump key been pressed within the last frame
    public Transform groundCheck; //need to initialize in the inspector!!!!!!!!!!!!!!
    public LayerMask groundLayer; //need to initialize in the inspector
    public float groundCheckRadius = 0.2f; //probably need to modify


    //inspector is the second way to initialize a value

    // Start is called before the first frame update
    private void Start()
    {
        facingRight = false; //final way to initialize, overrides all prior settings
        currentHeroState = HeroState.idle;

        //INitialize the animator variable - it's a component on THIS gameObject
        animator = GetComponent<Animator>(); //<T>  //call a method - make connection to component on gameObject in Unity scene
        animator.SetInteger( "HeroState",  (int) HeroState.idle      );

        rb2D = GetComponent<Rigidbody2D>();

    }

    //Fixed update used for physics, to give smooth motion, called at consistent time increments
    void FixedUpdate()
    {
        float inputX = Input.GetAxis("Horizontal"); // key input:  -1, 0, 1
        bool isWalking = Mathf.Abs(inputX) > 0; //convenient static method
        grounded = Physics2D.OverlapCircle( groundCheck.position , groundCheckRadius,groundLayer );
        bool jumpPressed = Input.GetButtonDown("Jump");

        if (isWalking)
        {
            if (inputX > 0 && !facingRight)
            {
                Flip();
            } else if( inputX < 0 && facingRight)
            {
                Flip();
            }
            currentHeroState = HeroState.walk;
            ///Move the player using 2D Physics - RigidBody allows movement
            rb2D.velocity = new Vector2(0, rb2D.velocity.y);////update his velocity, zero out x velocity 
            rb2D.AddForce(new Vector2(inputX * forceX, 0));
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


