###Class PlayerController

This example code works for an Animator Controller with 4 States as listed in the enum: heroState.

This code assumes  that there are prefab objects with tags:  
Collectable, Hazard

The prefabs must have the PickUp script on them.

```java

using System.Collections;
using System.Collections.Generic;
using UnityEngine;
using UnityEngine.SceneManagement;


public class PlayerController : MonoBehaviour
{

	private enum heroState
	{
		idle=0,
		walk=1,
		jump = 2,
        dead =3
	};

	private Rigidbody2D rb2D;
	private Animator anim;
	
	public float forceX;
	public float jumpForce;
	private bool jump;
	private bool facingRight;
    private bool dead;


	// Use this for initialization
	void Start ()
	{
        forceX = 80;
        jumpForce = 6;
		
        rb2D = GetComponent<Rigidbody2D> ();
		anim = GetComponent<Animator> ();
        anim.SetInteger("HeroState", (int)heroState.idle);
		
        facingRight = true;
		jump = false;
        dead = false;
	}

    //every frame, check to see if Jump button has been pressed
	void Update ()
	{
        if (Input.GetButtonDown("Jump")) //spacebar
        {
            jump = true;
        }
        else
        {
            jump = false;
        }

	}

    /// <summary>
    /// //move the player 
    /// </summary>
	void FixedUpdate ()
   {
        if (!dead)  //stop movement
        {   
		float inputX = Input.GetAxis ("Horizontal"); //arrows, a,d, 
		bool isWalking = Mathf.Abs (inputX) > 0; //we need to move the player

		if (isWalking) {
            
			anim.SetInteger ("HeroState", (int)heroState.walk);//1
			if (inputX > 0 && !facingRight) {
				flip ();
				Debug.Log ("Flipped to face Right");
			}
			if (inputX < 0 && facingRight) {
				flip ();
			}
            //set x velocity to 0, keep y velocity the same
			rb2D.velocity = new Vector2 (0, rb2D.velocity.y);

			rb2D.AddForce (new Vector2 (forceX * inputX, 0));  //add x force

		} else {
            anim.SetInteger ("HeroState", (int)heroState.idle);
		}

		if (jump) {
			
            anim.SetInteger ("HeroState", (int)heroState.jump);
            //set y velocity to 0, maintain x velocity when jumping
			rb2D.velocity = new Vector2 (rb2D.velocity.x, 0);
			rb2D.AddForce (new Vector2 (0f, jumpForce), ForceMode2D.Impulse);  //add force in Y direction
		  }
        }//end of if dead
       

	}

	void OnTriggerEnter2D (Collider2D hitObject)
	{
		if (hitObject.CompareTag ("Collectable")) {
			
            PickUp item = hitObject.GetComponent<PickUp>();
            if(item != null){
                GameData.instanceRef.Add(item);
                Debug.Log("Added item " + item.type);
            }
            Destroy(hitObject.gameObject);
		}
		if (hitObject.CompareTag ("Hazard")) {
            anim.SetInteger("HeroState", (int)heroState.dead);
	
            PickUp item = hitObject.GetComponent<PickUp>();
            if (item != null)
            {
                GameData.instanceRef.TakeDamage(item.value);
                Debug.Log("TakeDamage " + item.value);
               
            }
            Destroy(hitObject.gameObject);
            dead = true;
		}
	}

	void flip ()
	{
		facingRight = !facingRight;
		Vector3 theScale = transform.localScale;
		theScale.x *= -1;
		transform.localScale = theScale;
	}  

}

```

