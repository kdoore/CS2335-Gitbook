#PickUp, GameData, PlayerController Mods

The following scripts contain updated code required for integrating the new GameData Inventory inventory, from the new Inventory System.  

###Class Pickup


```java
using System.Collections;
using System.Collections.Generic;
using UnityEngine;


//combines frontEnd UI and BackEnd 
public class PickUp : MonoBehaviour {

    public ItemInstance itemInstance;

    /// <summary>
    /// Adds the item to GameData Inventory
    /// Can be executed by button.onClick
    /// when added as a listener
    /// </summary>
    public void AddItem( ) //can be called onClick for a button to add an item to inventory
    {
        //updated preferred method 
        GameData.instanceRef.AddItem(this.itemInstance);
        
        //removed 
        //GameData.instanceRef.Add(this.itemInstance);

    }
}
```


## GameData Changes:  with Inventory, Add(ItemInstance)
See changes in code below:

```java
//New Variable: 
public Inventory inventory;
//New Method:  
public void AddItem( ItemInstance){     }

```

##Updated Class GameData with Inventory, Add(ItemInstance)

```java
using System.Collections.Generic;
using UnityEngine;
using UnityEngine.Events;

//Singleton - one and only 1 ever in existance
//global variable to allow easy access
public class GameData : MonoBehaviour {


    /// static means it belongs to the Class, and not to an object instance of the class
    public Inventory inventory; ///Set in the inspector in BeginScene

    public static GameData instanceRef;  ///Global variable 

    public UnityEvent onPlayerDataUpdate = new UnityEvent();//calling constructor for our UnityEvent object

    private int score;
    private int health;

   
    //Properties - Support Encapsulation - protect inner workings of our class
    public int Score{
        get{ return score;    }   //read only access
      }

    public int Health
    {
        get { return health; }   //read only access
    }

    private void Awake()
    {
        health = 100; //initialize
        score = 0;

        if(instanceRef == null){
            instanceRef = this; //point to itself
            DontDestroyOnLoad(this.gameObject);  //this will never be destroyed
        }else{
            Destroy(this.gameObject);
            Debug.Log("Duplicate GameData is Destroyed");
        }
    }

    public void Add( int value){
        score += value;
        Debug.Log("Score is updated " + score);
        //Score has changed
        if(onPlayerDataUpdate != null)// there are no listeners, the list is empty
        {
            onPlayerDataUpdate.Invoke(); //Broadcast the event ( execute the listener methods)
         //Invoke here means execute all methods on the listner list.
        }
    }

    //Adds an item to the inventory - list
    //remove this and use the method below
    //inventory publishes the onInventoryUpdated event
    public void Add(ItemInstance item)    
    {
    inventory.InsertItem(item);
    Debug.Log("Add an item to the inventory " +         item.item.itemName);
    }

    //Preferred - Adds an item to the inventory - list
    public void AddItem(ItemInstance item)
    {
        inventory.InsertItem(item);
        Debug.Log("Add an item to the inventory " + item.item.itemName);
    }


    public void TakeDamage( int value){
        health -= value;
        Debug.Log("Health is updated " + health);
    }

    public void ResetGameData()
    {
        health = 100; //initialize
        score = 0;
    }

} //end class
```

#Updated code for PlayerController
The code below has been updated in onTriggerEnter.

We needed to change the code that accessed the value field of each PickUp, since the PickUp class now has an ItemInstance object that contains that value data.

- `Add(item.value)` is changed to `Add(item.itemInstance.value)`

We also added the gameData method: `AddItem( item.itemInstance )`, so that we add the item to the GameData inventory.
 
 ###Code - PlayerController

```java
 
using System.Collections;
using System.Collections.Generic;
using UnityEngine;


public class PlayerController : MonoBehaviour {

    public enum HeroState { idle, walk, jump, dead      }


    private Rigidbody2D rb2D;  //declare the variable
    public float forceX = 50f;  //this is probably too small
    private bool facingRight;


    public bool grounded; //tracks if player is touching ground
    private bool jump;  //has jump key (spacebar been pressed - last)
    public Transform groundCheck; //set in inspector, (add child to player - empty gameObject at player's feet)
    public LayerMask groundLayer;  //set in inspector
    public float groundCheckRadius = 0.2f;  //modify as needed
    public float jumpForce = 8f; //modify as needed


    private Animator animator;

	// Use this for initialization
	void Start () {
        rb2D = GetComponent< Rigidbody2D >();   ///we made the connection with the component
        facingRight = true;
        animator = GetComponent<Animator>();
        animator.SetInteger("HeroState",(int) HeroState.idle);

    }
	
	//Fixed Update is called at regular time intervals - use with Physics2D
	void FixedUpdate () {
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

        if ( isWalking){

            if( inputX > 0 && !facingRight){
                Flip();
            }
            else if(inputX < 0 && facingRight){
                Flip();
            }
            rb2D.velocity = new Vector2(0, rb2D.velocity.y);
            rb2D.AddForce(new Vector2(inputX * forceX , 0));
            animator.SetInteger("HeroState", (int)HeroState.walk);
        } //end if isWalking
        else
        {
            animator.SetInteger("HeroState", (int)HeroState.idle);
        } //end else

        if( jump && grounded)
        {
            animator.SetInteger("HeroState", (int)HeroState.jump);
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

            //update score
            GameData.instanceRef.Add(item.itemInstance.value); //points for each specific item's value

            //add to inventory
            GameData.instanceRef.AddItem(item.itemInstance);

            Debug.Log("Hit collectible");
            Destroy(collision.gameObject);
        }
        else if (collision.CompareTag("Hazard"))
        {
            //decrease health
            PickUp item = collision.GetComponent<PickUp>();
            GameData.instanceRef.TakeDamage(item.itemInstance.value);

            Debug.Log("Hit Hazard: value is " + item.itemInstance.value);
            Destroy(collision.gameObject);
        }
        else
        {
            Debug.Log("Hit Something Else");
        }
    } //end function

    private void Flip(){
        facingRight = !facingRight; //toggle this value
        Vector3 theScale = transform.localScale;
        theScale.x *= -1;  //we have changed the value
        transform.localScale = theScale;

    }

} //end Class


```

