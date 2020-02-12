#PlayerController with Jumping

In order to use the following script to enable your player to jump, the following changes must be configured in Unity before the following script will work.

**Unity Configuration Steps:**
 - Create a new **Layer:** **Ground** in the Unity Layer Editor.  All gameObjects which will act as ground, (example: Floor) to allow jumping, must have this layer set in the inspector. See image below

![](/assets/Screen Shot 2019-02-12 at 11.15.18 AM.png)

 - Set the **Layer: Ground **for all gameObjects that the player should be able to jump from.  Example:  Floor 
 
![](/assets/Screen Shot 2019-02-12 at 11.18.11 AM.png)

    
 - Create an empty gameObject, make it a child of the Player, name: **GroundCheck**. Add an icon (orange capsule in the image ), so it's easy to see.  Move the GroundCheck gameObject to the bottom of the player's feet, this gameObject will check to see if it's in contact with a gameObject that is in the 'Ground' layer.  
    
    
![](/assets/Screen Shot 2019-02-12 at 11.26.14 AM.png)

###PlayerController - with jump Code

```java
using System.Collections;
using System.Collections.Generic;
using UnityEngine;

[DisallowMultipleComponent] //only one of these scripts allowed on a gameObject
public class PlayerController : MonoBehaviour {

    private Rigidbody2D myRBody2D;
    public float forceX=80f;
    private bool facingRight; 
   
   //add these new variables  
    public bool grounded; //tracks if player is touching ground
    private bool jump;  //has jump key (spacebar been pressed - last)
    public Transform groundCheck; //set in inspector, (add child to player - empty gameObject at player's feet)
    public LayerMask groundLayer;  //set in inspector
    public float groundCheckRadius = 0.2f;  //modify as needed
    public float jumpForce=8f; //modify as needed

    void Start()
    {
        myRBody2D = GetComponent<Rigidbody2D>();
        facingRight = true;

    }

  
    void FixedUpdate()
    {
        float inputX = Input.GetAxis("Horizontal");
        bool isWalking = Mathf.Abs(inputX) > 0;

        grounded = Physics2D.OverlapCircle(groundCheck.position, groundCheckRadius, groundLayer); //draws a small circle to check for intersection with a gameObject on Ground Layer
        
        //to use w and up arrow use the following:
        /*
        float inputY = Input.GetAxis("Vertical");
        bool jumpPressed = inputY > 0;
        */
        bool jumpPressed = Input.GetButtonDown("Jump"); //spacebar was last key pressed
        if (jumpPressed)
        {
            jump = true;
        }
        else
        {
            jump = false;
        }

        if (isWalking)
        {
            if (inputX > 0 && !facingRight)
            {
                flip();
                Debug.Log("flip right");
            }
            if (inputX < 0 && facingRight)
            {
                flip();
                Debug.Log("flip left");
            }
            myRBody2D.velocity = new Vector2(0, myRBody2D.velocity.y); //zero out velocity.x, maintain velocity.y
            // reset velocity to 0
            myRBody2D.AddForce(new Vector2(forceX * inputX, 0));
        } // end if (isWalking)
        if (jump && grounded)//must also be grounded in order to jump
        {
            myRBody2D.velocity = new Vector2(myRBody2D.velocity.x, 0); //zero out velocity.y, maintain velocity.x
            myRBody2D.AddForce(new Vector2(0f, jumpForce), ForceMode2D.Impulse); //add force as impulse
        } // end if (jump)

    }

    //Customize to your game needs
    ////This is the EVENT that DRIVES the MiniGame, Player colliding with Pickup Objects
    void OnTriggerEnter2D(Collider2D hitObject)
    {
        if (hitObject.CompareTag("Collectible"))
        {
            Debug.Log("Hit Collectible");
            PickUp item = hitObject.GetComponent<PickUp>();
            GameData.instanceRef.Add(item.value);
            Destroy(hitObject.gameObject);
        }
        else if (hitObject.CompareTag("Hazard"))
        {
            Debug.Log("Hit Hazard");
            PickUp item = hitObject.GetComponent<PickUp>();
            GameData.instanceRef.TakeDamage(item.value);
            Destroy(hitObject.gameObject);
        }
        else
        {
            Debug.Log("collided with some other object");
        }
    } //end OnTriggerEnter2D

   

    void flip()
    {
        facingRight = !facingRight;
        Vector3 theScale = transform.localScale;
        theScale.x *= -1;
        transform.localScale = theScale;
    }

   
} //end class

```

