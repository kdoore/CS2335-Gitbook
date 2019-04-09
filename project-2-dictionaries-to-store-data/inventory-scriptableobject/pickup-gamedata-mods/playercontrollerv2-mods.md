#PlayerController_v2 Mods

Below is the updated code, so that it works with the new GameData Inventory for the simplified PlayerController_v2 script included with the simplified MiniGame provided here: 



```java

public class PlayerController_v2 : MonoBehaviour {

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
	void Start () {
        rb2D = GetComponent< Rigidbody2D >();   ///we made the connection with the component
        facingRight = true;
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
                   } //end if isWalking
       
        if( jump && grounded)
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

            //updating the score
            GameData.instanceRef.Add(item.itemInstance.value); //points for each specific item's value
            //add to inventory
            GameData.instanceRef.Add(item.itemInstance);

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
