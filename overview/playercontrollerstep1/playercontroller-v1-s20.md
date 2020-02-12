#PlayerController - v1 - Spring 2020


Includes fogic for:  
    - Animator: idle, walk, jump
    - RigidBody: horizontal movement
    - Flip (): flip horizontally to face walk direction
    
Does not include logic for:  
    
    - RigidBody: vertical movement
    - Jump: Grounded, Physics LayerMask logic
    - OnTriggerEnter( ) - collision with pickUp objets
    
###Code - PlayerController  Version 1, Spring 2020

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

    // Start is called before the first frame update
    void Start()
    {
        currentHeroState = HeroState.idle;  //we can see the current value in inspector
        animator = GetComponent<Animator>(); //initialize by connection to gameObject component
        animator.SetInteger("HeroState", (int)HeroState.idle); //misspelling will give run-time error in Unity
        facingRight = true;
        myRBody2D = GetComponent< Rigidbody2D >();
        forceX = 100.0f;
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
            myRBody2D.velocity = new Vector2(0, 0);  //set velocity to 0 x, y
            myRBody2D.AddForce(new Vector2(forceX * inputX, 0));  //
        }
        else 
        { //set back to Idle, if it was walking,jumping in last frame
            animator.SetInteger("HeroState", (int)HeroState.idle);
            currentHeroState = HeroState.idle;
        }

        bool jumpPressed = Input.GetButtonDown("Jump"); //is spacebar pressed
        if (jumpPressed)
        {
            animator.SetInteger("HeroState", (int)HeroState.jump);
            currentHeroState = HeroState.jump;
            ///ADD VERTICAL MOVEMENT LOGIC
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

