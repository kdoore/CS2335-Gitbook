#PlayerController.cs V2 Sept 16

This version of PlayerController includes code for Animation, and Movement of the player game object.

Required GameObjects:  
    - **Player** with Animator Component, Rigidbody2D, Collider2D

    - **GroundCheck:**  Empty GameObject placed as a **child of the Player**, transform positioned at the feet of the Player.

    - **Floor**  Empty GameObject with BoxCollider2D, Layer: Ground


![](/assets/Screen Shot 2019-09-17 at 12.53.27 PM.png)

![](/assets/Screen Shot 2019-09-17 at 12.58.40 PM.png)

![](/assets/Screen Shot 2019-09-17 at 1.00.03 PM.png)

**CODE: PlayerController.cs**


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
    public Transform groundCheck; //need to initialize in the inspector!! (set to GroundCheck gameObject)
    public LayerMask groundLayer; //need to initialize in the inspector!!  (Set as 'Ground' Layer)
    public float groundCheckRadius = 0.2f; //probably need to modify


    //inspector is the second way to initialize a value

    // Start is called before the first frame update
    private void Start()
    {
        facingRight = true; //final way to initialize, overrides all prior settings
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

