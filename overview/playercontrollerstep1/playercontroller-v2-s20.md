PlayerController V2  Feb 2020


GroundCheck GameObject must be configured as child of player
Physics Layer:  Ground must be configured




```java

using System.Collections;
using System.Collections.Generic;
using UnityEngine;

public class PlayerController : MonoBehaviour
{
    public enum HeroState {idle, walk, jump }    //CONSTANT:  Custom data-type - integer using words

    public HeroState currentHeroState; //instance of a variable using our custom data-type

    private Animator animator;  //object reference variable - null, null reference exception - initialization error
    public bool facingRight;   //default is that this is false, facing left
    private Rigidbody2D myRBody2D;

    public float forceX; //stores force for movement in horizontal direction

    public float jumpForce = 10f;
    public Transform groundCheck;  //transform component on the GroundCheck gameObject
    public LayerMask groundLayer; //Layer that the groundCheck is checking for contact for jumping
    public float groundCheckRadius = 0.5f; //modify as needed
    public bool grounded;  //is it grounded

    // Start is called before the first frame update
    void Start()
    {
        currentHeroState = HeroState.idle;  //we can see the current value in inspector
        animator = GetComponent<Animator>(); //initialize by connection to gameObject component
        animator.SetInteger("HeroState", (int)HeroState.idle); //misspelling will give run-time error in Unity
        facingRight = true;
        myRBody2D = GetComponent< Rigidbody2D >();
        forceX = 100.0f;
        groundCheckRadius = 0.5f;
    }

    // FixedUpdate is called with constant-time between each execution
    void FixedUpdate()
    {
        float inputX = Input.GetAxis("Horizontal"); //-1, 0, 1 values for L,R arrows, a, d
        bool isWalking = Mathf.Abs(inputX) > 0;  //left or right walking movement

        if (isWalking)
        {
            animator.SetInteger("HeroState", (int)HeroState.walk); //send signal to animator
            currentHeroState = HeroState.walk;

            if ( facingRight && inputX < 0) //facing right, moving left
            {
                Flip(); //flip to make facing left
            }
            if( !facingRight && inputX > 0) //facing left, moving right
            {
                Flip(); //flip to set facingRight
            }
            ////ADD MOVEMENT LOGIC - HORIZONTAL
            myRBody2D.velocity = new Vector2(0, myRBody2D.velocity.y);  //set velocity to 0 x, keep y velocity
            myRBody2D.AddForce(new Vector2(forceX * inputX, 0));  //
        }
        else 
        { //set back to Idle, if it was walking,jumping in last frame
            animator.SetInteger("HeroState", (int)HeroState.idle);
            currentHeroState = HeroState.idle;
        }

        bool jumpPressed = Input.GetButtonDown("Jump"); //is spacebar pressed
        grounded = Physics2D.OverlapCircle(groundCheck.position, groundCheckRadius, groundLayer);

        if (jumpPressed  && grounded)
        {
            animator.SetInteger("HeroState", (int)HeroState.jump);
            currentHeroState = HeroState.jump;
            ///ADD VERTICAL MOVEMENT LOGIC
            myRBody2D.velocity = new Vector2(myRBody2D.velocity.x, 0);  //set velocity to 0 y, keep x velocity
            myRBody2D.AddForce(new Vector2(0,jumpForce),ForceMode2D.Impulse);  //
        }

    } //end FixedUpdate

    private void Flip()
    {
        facingRight = !facingRight; //toggle to opposite value 
        //  transform is a variable initialized to refer to the current gameObject's transform component
        //Common pattern when working with vector values in Unity
        Vector3 theScale = transform.localScale; //initialize temp Vector3 with current transform values
        theScale.x *= -1; //multiple current Scale.x by -1
        transform.localScale = theScale;
    }

} //end PlayerController

```

