#PlayerController_v2 Final Code
**Important:  This code is to be used with the Simplified MiniGame.**  In **LevelManager, the script component of type: PlayerController is used, you must make a change to LevelManager**, so it uses PlayerController_v2, or change the name of this file, and class to PlayerController.cs


Includes Updates for:  InventorySystem, LevelManager.

**OnTriggerEnter2D** contains code to test for collisions with GameObjects with Collider2D set as 'Trigger' based on several different gameObject tags
** Tags used in PlayerController **:
 - **Collectible**
     - Requires either PickUp.cs or ScorePickUp.cs
     - PickUp.cs requires ItemInstance - ScriptableObject - adds value to Score, adds ItemInstance to Inventory
     - ScorePickUp.cs - adds value to score
     
 - **Hazard**
     - Requires either PickUp.cs or Hazard.cs
 - **Water**
     - Invokes: onPlayerDied
 - **Exit**
     - Invokes: onReachedExit

Audio clips played if colliding with Pickup with correct audioSource.  ( Collectible, Water )

Audio Clips:  


Contains custom Events, LevelManager is the subscriber object:  
- onPlayerReachedExit
- onPlayerDied 
 

Updated Apr 22, 2019

```java
using System.Collections;
using System.Collections.Generic;
using UnityEngine;
using UnityEngine.Events;


/// Player controller v2.
/// Code provided 
/// Sp.2019

public class PlayerController_v2 : MonoBehaviour
{
    public UnityEvent onPlayerReachExit = new UnityEvent();
    public UnityEvent onPlayerDied = new UnityEvent();
    public Transform spawnPoint; //player respawn position for each new level


    private Rigidbody2D rb2D;  //declare the variable
    public float forceX = 50f;  //this is probably too small
    private bool facingRight;

    public bool grounded; //tracks if player is touching ground
    private bool jump;  //has jump key (spacebar been pressed - last)
    public Transform groundCheck; //set in inspector, (add child to player - empty gameObject at player's feet)
    public LayerMask groundLayer;  //set in inspector
    public float groundCheckRadius = 0.2f;  //modify as needed
    public float jumpForce = 8f; //modify as needed

    // Use this for initialization
    void Start()
    {
        gameObject.transform.localPosition = spawnPoint.localPosition;
        rb2D = GetComponent<Rigidbody2D>();   ///we made the connection with the component
        facingRight = true;
    }

    //Fixed Update is called at regular time intervals - use with Physics2D
    void FixedUpdate()
    {
        float inputX = Input.GetAxis("Horizontal");  //-1, 0, 1
        bool isWalking = Mathf.Abs(inputX) > 0;

        grounded = Physics2D.OverlapCircle(groundCheck.position, groundCheckRadius, groundLayer);

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
                Flip();
            }
            else if (inputX < 0 && facingRight)
            {
                Flip();
            }
            rb2D.velocity = new Vector2(0, rb2D.velocity.y);
            rb2D.AddForce(new Vector2(inputX * forceX, 0));
        } //end if isWalking

        if (jump && grounded)
        {
            rb2D.velocity = new Vector2(rb2D.velocity.x, 0); //zero out velocity.y, maintain velocity.x
            rb2D.AddForce(new Vector2(0f, jumpForce), ForceMode2D.Impulse); //add force as impulse

        }

    }

    /// <summary>
    /// THIS IS THE EVENT that starts the chain reaction of events
    /// </summary>
    /// <param name="collision">Collision.</param>
    //Customize to your game needs
    private void OnTriggerEnter2D(Collider2D collision)
    {
        if (collision.CompareTag("Collectible"))
        {
            //update score
            PickUp item = collision.GetComponent<PickUp>();
            if (item != null)
            {
                GameData.instanceRef.Add(item.Value); //points for each specific item's value

                //add to inventory
                GameData.instanceRef.AddItem(item.itemInstance); //points for each specific item's value
            }
            else //is it a scoreItem
            {
                ScorePickUp scoreItem = collision.GetComponent<ScorePickUp>();
                if (scoreItem != null)
                {
                    GameData.instanceRef.Add(item.Value); //points for each specific item's value
                }
            }
            AudioSource collectSound = collision.gameObject.GetComponent<AudioSource>();
            if (collectSound != null)
            {
                AudioClip clip = collectSound.clip;
                //plays a clip at a point in worldspace, destroyed when done
                AudioSource.PlayClipAtPoint(clip, new Vector3(5, 1, 2));
            }

            Destroy(collision.gameObject);
        }
        else if (collision.CompareTag("Hazard"))
        {
            //decrease health
            Hazard hItem = collision.GetComponent<Hazard>();
            if (hItem != null)
            {
                GameData.instanceRef.TakeDamage(hItem.Value);
            }
            else //PickUp with Hazard, not added to Inventory here
            {
                PickUp item = collision.GetComponent<PickUp>();
                if (item != null)
                {
                    GameData.instanceRef.TakeDamage(item.Value);
                }
                Debug.Log("Hit Hazard: value is " + item.Value);
            }
            Destroy(collision.gameObject);
        }
        else if (collision.CompareTag("Water"))
        {
            Debug.Log("Hit Water");
            AudioSource diedSound = collision.gameObject.GetComponent<AudioSource>();
            if (diedSound != null)
            {
                AudioClip clip = diedSound.clip;
                //plays a clip at a point in worldspace, destroyed when done
                AudioSource.PlayClipAtPoint(clip, new Vector3(5, 1, 2));
            }
            if (onPlayerDied != null) //there are listeners
            {
                onPlayerDied.Invoke();
            }
        }
        else if (collision.CompareTag("Exit"))
        {
            Debug.Log("Hit Exit");
            onPlayerReachExit.Invoke();
        }
        else
        {
            Debug.Log("Hit Something Else");
        }
    } //end function

    private void Flip()
    {
        facingRight = !facingRight; //toggle this value
        Vector3 theScale = transform.localScale;
        theScale.x *= -1;  //we have changed the value
        transform.localScale = theScale;

    }

} //end Class


```

