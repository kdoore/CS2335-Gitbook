#Sept 23 Code
PlayerController
  - Updates for OnTriggerEnter2D for Collisions with collectibles and hazards.
  
  using System.Collections;
using System.Collections.Generic;
using UnityEngine;



```java
public class PlayerController : MonoBehaviour
{
    //create custom data-type
    public enum HeroState { idle, walk , jump }

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


        if (jumpPressed && grounded)
        {
            currentHeroState = HeroState.jump;
            animator.SetInteger("HeroState", (int)HeroState.jump);
            rb2D.velocity = new Vector2( rb2D.velocity.x, 0);////update his velocity, zero out x velocity 
            rb2D.AddForce(new Vector2(0, jumpForce),ForceMode2D.Impulse);

        }

    }
    /// <summary>
    /// Unity Event Handler Method
    /// IMPORTANT METHOD _ MAIN GAME CODE HERE _ START OF CHAIN OF EVENTS
    /// </summary>
    /// <param name="collision">Collision.</param>
    private void OnTriggerEnter2D(Collider2D collision)
    {
        //Debug.Log("Entered Trigger");
        if (collision.CompareTag("Collectible")){
            Debug.Log("Hit collectible");
            PickUp item = collision.GetComponent<PickUp>();
            GameData.instanceRef.Add(item.value);
            Destroy(collision.gameObject); //destroy collision component
        
        
        }else if (collision.CompareTag("Hazard"))
        {
            Debug.Log("Hit Hazard");
            PickUp item = collision.GetComponent<PickUp>();
            GameData.instanceRef.TakeDamage(item.value);
            Destroy(collision.gameObject); //destroy gameObject
        }
        else
        {
            Debug.Log("Hit Something else");
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

