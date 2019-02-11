#PlayerController with Jumping



```java
using System.Collections;
using System.Collections.Generic;
using UnityEngine;

[DisallowMultipleComponent] //only one of these scripts allowed on a gameObject
public class PlayerController : MonoBehaviour {

    private Rigidbody2D rb2D;
    public Transform groundCheck; //add child to player - empty gameObject at player's feet
    public float forceX=80f;
    public float jumpForce=8f;
    private bool facingRight; 
    public bool grounded;
    private bool jump;
    public LayerMask groundLayer;
    public float groundCheckRadius = 0.2f;

    void Start()
    {
        rb2D = GetComponent<Rigidbody2D>();
        facingRight = true;

    }

  
    void FixedUpdate()
    {
        float inputX = Input.GetAxis("Horizontal");
        bool isWalking = Mathf.Abs(inputX) > 0;

        grounded = Physics2D.OverlapCircle(groundCheck.position, groundCheckRadius, groundLayer);
        if (Input.GetButtonDown("Jump")) //spacebar
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
            rb2D.velocity = new Vector2(0, rb2D.velocity.y);
            // reset velocity to 0
            rb2D.AddForce(new Vector2(forceX * inputX, 0));
        }
        if (jump && grounded)
        {
            rb2D.velocity = new Vector2(rb2D.velocity.x, 0);
            rb2D.AddForce(new Vector2(0f, jumpForce), ForceMode2D.Impulse);
        }

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

