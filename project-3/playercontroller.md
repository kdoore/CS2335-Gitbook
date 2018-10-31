###Class PlayerController

This example code works for an Animator Controller with 4 States as listed in the enum: heroState.

See [Animator Controller](/animation_controller.md) details: 

This code assumes  that there are prefab objects with **tags:  **
**Collectible, Hazard**

The prefabs must have the PickUp script on them.

This example assumes you have a GameData object.  For ease of testing, create an empty GameObject: GameManager, in the MiniGame scene.  Add the GameData script to the GameManager.  Also, in the BeginScene, add GameData to the GameManager GameObject ( it currently has StateManager attached)

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
   
	}

	void OnTriggerEnter2D (Collider2D hitObject)
	{
		if (hitObject.CompareTag ("Collectible")) {
			
            PickUp item = hitObject.GetComponent<PickUp>();
            if(item != null){
                GameData.instanceRef.Add(item);
                Debug.Log("Added item " + item.type);
                item.DestroyMe(); //have item destroy itse.f
            }
            //Destroy(hitObject.gameObject);
		}
		if (hitObject.CompareTag ("Hazard")) {
             if (GameData.instanceRef.Health <= 0)   ///play when health less than eq to 0
            {
                anim.SetInteger("HeroState", (int)heroState.dead);

            }
	
            PickUp item = hitObject.GetComponent<PickUp>();
            if (item != null)
            {
                GameData.instanceRef.TakeDamage(item.value);
                Debug.Log("TakeDamage " + item.value);
                item.DestroyMe();  //have item destroy itself

            }
            //Destroy(hitObject.gameObject);
          
		}
	}

	void flip ()
	{
		facingRight = !facingRight;
		Vector3 theScale = transform.localScale;
		theScale.x *= -1;
		transform.localScale = theScale;
	}  
	
	//this method is executed from a trigger at the last frame of the dead animation.
	 public void ReloadScene()
    {
         if (GameData.instanceRef.Lives > 1)
        {
            GameData.instanceRef.Lives -= 1; //loose a life each time to reload the scene
            StateManager.instanceRef.SwitchState(new MiniGameState());
            SceneManager.LoadScene("MiniGameScene");
        }

        else //end the game
        {
            StateManager.instanceRef.SwitchState(new EndState());
            SceneManager.LoadScene("EndScene");
        }
       

    }//end ReloadScene
}

```

