Code for Feb 10-11 2020

```java
using System.Collections;
using System.Collections.Generic;
using UnityEngine;

public class PlayerController : MonoBehaviour
{
    public enum HeroState { idle, walk, jump  }  //create custom data-type - have integer values

    public HeroState currentHeroState;  //will display the current enum 

    private Animator animator;  //- null - reference variable to access animator component

    public bool facingRight;  //keep track of sprite direction - used in Flip
    private Rigidbody2D myRBody2D;
    public float forceX; //used for adjusting velocity

    public Transform groundCheck; //transform component on GroundCheck object
    public LayerMask groundLayer; //allows us to interact with a physics Layer 
    public float groundCheckRadius ;  //
    public float jumpForce;
    public bool grounded = false;  //will let us see if the gameOjb is grounded

    // Start is called before the first frame update
    void Start()
    {
        currentHeroState = HeroState.idle;    //initialize to show it's in idle to start
        animator = GetComponent<Animator>();//is on the same game object as this script
        animator.SetInteger("HeroState", (int)HeroState.idle);  //send in the signal: 0
        facingRight = true;
        myRBody2D = GetComponent<Rigidbody2D>();
        forceX = 100.0f;  //force value may need adjusted

        groundCheckRadius = 0.2f; //may need modified
        jumpForce = 10f; //may need modified


    }

    // Update is called once per frame
    void FixedUpdate()  //physics methods executed - want consistant time between frames
    {
        float inputX = Input.GetAxis("Horizontal");  //values of -1, 0 , 1 
        bool isWalking = Mathf.Abs(inputX) > 0;

        if (isWalking)
        {
            //check for flipping
            if( inputX > 0 && !facingRight)  //moving right, facing left
            {
                Flip(); //flip right
            }else if( inputX <0 && facingRight) //moving left, facing right
            {
                Flip(); //flip left
            }
            animator.SetInteger("HeroState", (int)HeroState.walk);
            myRBody2D.velocity = new Vector2( 0, myRBody2D.velocity.y);  //reset the velocity to 0 //may come back and change
            myRBody2D.AddForce(new Vector2(inputX * forceX, 0)); //add horizontal force to move the player
        }
        else
        {
            animator.SetInteger("HeroState", (int)HeroState.idle);
        }

        bool jumpPressed = Input.GetButtonDown("Jump");   //is spacebar pressed
        grounded = Physics2D.OverlapCircle(groundCheck.position, groundCheckRadius, groundLayer);

        if (jumpPressed  && grounded)
        {
            animator.SetInteger("HeroState", (int)HeroState.jump);
            Debug.Log("Jumping");

            ///Vertical Force for Movement
            myRBody2D.velocity = new Vector2(myRBody2D.velocity.x, 0);  //reset the velocity to 0 //keep horizontal movement
            myRBody2D.AddForce(new Vector2(0,jumpForce),ForceMode2D.Impulse); //add horizontal force to move the player

        }

    }//end FixedUpdate

    //we have determined it is facing the wrong direction, it must need flipped for this to be executed
    private void Flip()
    {
        facingRight = !facingRight; //toggle direction variable
        //get Scale Vector
        Vector3 theScale = transform.localScale;  //get the current values for Scale Vector
        theScale.x *= -1; //modify the X component    //mirror the sprite
        transform.localScale = theScale; //set the actual Scale vector with our temp Vector
    }

    /// <summary>
    /// This is the start of the Event-Chain for Game Score, health to be changed
    /// </summary>
    /// <param name="collision">Collision.</param>
    private void OnTriggerEnter2D(Collider2D collision)
    {
        //check tag on gameObject that has this collider on it
        if (collision.CompareTag("Collectible"))
        {
            //get the PickUp component from the gameObject with this collider on it
            PickUp item = collision.GetComponent<PickUp>();
            Debug.Log("collided with a collectible of value: " +   item.Value);
        } 
        else if (collision.CompareTag("Hazard"))
        {
            PickUp item = collision.GetComponent<PickUp>();
            Debug.Log("collided with a hazard of value: " + item.Value);

        }
    }



}///end of Class

```

