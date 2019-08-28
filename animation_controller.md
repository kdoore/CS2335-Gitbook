# Animator Controller

The Unity Animator controller provides a visual programming language implementation of a state machine to control coordination of animations which can be attached to gameObjects.


**Animation Tutorials:  **

[Unity 2D Character Animation Tutorial - John Stejskal](http://johnstejskal.com/wp/creating-2d-animations-from-sprite-sheets-in-unity3d/)


**Recommended for creating simple animations**
[2D Animation in Unity - Raywenderlich](http://www.raywenderlich.com/66345/unity-2d-tutorial-animations)


[Animate UI Elements](https://www.raywenderlich.com/149955/introduction-unity-ui-part-2-2)

[Unity3D-Live Training: Animation Beginner Tutorial](https://unity3d.com/learn/tutorials/modules/beginner/live-training-archive/animate-anything)

[Link to Free - 2D GameArt - Sprites used here](http://www.gameart2d.com/freebies.html)

**In Class Code Example:**
 TODO: add code for fall 19



### Create an Animation from a series of sprites

To create an animation, we can select a series of sprites and drag them into the scene window.  We are then prompted to save this animation.  This immediately creates several components:

* Animation clip:   ![](/assets/Screen Shot 2018-03-22 at 12.26.09 PM.png)

* Animator controller:  ![](/assets/Screen Shot 2018-03-22 at 12.27.35 PM.png)

* Animator component  ![](/assets/Screen Shot 2018-03-22 at 12.27.51 PM.png)

In addition, in order to integrate animations with our Unity program, we will probably want to use the following additional items:

* Animator parameters
* Custom C\# controller script
* GameObject with RigidBody2D - used for GameObjects that will be moved by the Physics System.
* GameObject with Collider2D - to detect collisions with other GamObjects to: earn points, lose health, get collectable objects.

### Animation Clip

In 2D mode, we can create animation clips using sprites.  We can use a series of sprites, where each sprite is in a slightly different position, so that when we show these sprites in a sequential loop, we see animated behavior.  Or, we can use a single sprite, and use the Animation editor to modify some feature of the sprite over time using the dope-sheet or curves editor.

### Animator Controller

The Animator Controller, Mechanim, is a visual interface for creating a state machine to control animations in Unity.  It has the ability to utilize input parameters as event signals, these input parameters provide the connection between the Mechanim state-machine and custom C\# scripts.   The animation controller has a set of nodes that correspond to states, it is easy to add arc - transitions between nodes. Each transition can be configured to use any number of conditions to determine when a transition is executed to change the current state of the system. There are also many online tutorial examples that show how to use Mechanim as a state-machine to control game-logic, instead of animations. 

### Animator Component

In order to integrate an animation with a gameObject, we need a 2D gameObject that has a Sprite Renderer, then we can add an Animator Component from the component miscellaneous tab.  Then select the empty animator-controller section in the inspector to select an animation controller from your assets folder.


### Custom Player-Controller Script

To control a animation associated with a gameObject, outside of the Animator Controller, such as user-input, then we'll need a custom C\# script that can manage the logic to determine what input events have occurred and whether the input should impact the animation controller state.  Our custom script will send input signals to the Animator Controller State Machine, where the animator controller state machine will consider the current animator state, the input signals, and the defined transition conditions to determine whether to switch to a new animation clip.

Below is a simple example class: PlayerController.cs which checks for left-right arrow keyboard input, or the fire1 key which maps to the control key on a MAC.  In Fixed-Update, the code checks for input, then uses if-else statement blocks to determine what value to send to the Animator component that is attached to the gameObject. Below is the link to a set of free sprites including the cat sprites used in the starter code example animations.

[Link to Free - 2D GameArt - Cat Sprite Assets](http://www.gameart2d.com/freebies.html)


```
using System.Collections;
using System.Collections.Generic;
using UnityEngine;
using UnityEngine.SceneManagement;


public class PlayerController : MonoBehaviour
{

    private enum heroState
    {
        idle = 0,
        walk = 1,
        jump = 2,
        dead = 3
    };

    private Rigidbody2D rb2D;
    private Animator anim;
    public float forceX;
    public float jumpForce;
    private bool jump;
    private bool facingRight;
    private bool dead;


    // Use this for initialization
    void Start()
    {
        forceX = 80;
        jumpForce = 6;
        rb2D = GetComponent<Rigidbody2D>();
        anim = GetComponent<Animator>();
        anim.SetInteger("HeroState", (int)heroState.idle);
        facingRight = true;
        jump = false;
        dead = false;
    }

    //every frame, check to see if Jump button has been pressed
    void Update()
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
    void FixedUpdate()
    {
            float inputX = Input.GetAxis("Horizontal"); //arrows, a,d,
            bool isWalking = Mathf.Abs(inputX) > 0; //we need to move the player

            if (isWalking)
            {
                anim.SetInteger("HeroState", (int)heroState.walk);//1
                if (inputX > 0 && !facingRight)
                {
                    flip();
                    Debug.Log("Flipped to face Right");
                }
                if (inputX < 0 && facingRight)
                {
                    flip();
                }
                //set x velocity to 0, keep y velocity the same
                rb2D.velocity = new Vector2(0, rb2D.velocity.y);

                rb2D.AddForce(new Vector2(forceX * inputX, 0)); //add x force

            }
            else
            {
                anim.SetInteger("HeroState", (int)heroState.idle);
            }

            if (jump)
            {
                anim.SetInteger("HeroState", (int)heroState.jump);
                //set y velocity to 0, maintain x velocity when jumping
                rb2D.velocity = new Vector2(rb2D.velocity.x, 0);
                rb2D.AddForce(new Vector2(0f, jumpForce), ForceMode2D.Impulse); //add force in Y direction
            }
    }

    void OnTriggerEnter2D(Collider2D hitObject)
    {
        if (hitObject.CompareTag("Collectible"))
        {
            PickUp item = hitObject.GetComponent<PickUp>();
            if (item != null)
            {
              //  GameData.instanceRef.Add(item);
                Debug.Log("Added item " + item.type);
                item.DestroyMe(); //better way to have item destroy itself
                //Destroy(hitObject.gameObject);     //one way to destroy object
            }

        }
        if (hitObject.CompareTag("Hazard"))
        {
            PickUp item = hitObject.GetComponent<PickUp>();
            if (item != null)
            {
                //GameData.instanceRef.TakeDamage(item.value);
                Debug.Log("TakeDamage " + item.value);
                item.DestroyMe(); //have item destroy itself

            }
            //Destroy(hitObject.gameObject);
           // dead = true;
        }
    }

    void flip()
    {
        facingRight = !facingRight;
        Vector3 theScale = transform.localScale;
        theScale.x *= -1;
        transform.localScale = theScale;
    }

}

```

###Additional Unity 2D Animation Links:

[Sprite Sheet Animation Tutorial ](http://michaelcummings.net/mathoms/creating-2d-animated-sprites-using-unity-4.3)

John Stejskal, [IndieGameBuzz 3-Part tutorial](http://indiegamebuzz.com/create-2d-sprite-based-animation-states-in-unity3d/)

[rm2kdev:](https://www.youtube.com/watch?v=TU6wflRqT5Q) Unity Animator Tutorial  with Player Sprite Sheet

2D Character Animation Sprites - [2D GameArt](http://www.gameart2d.com/freebies.html)

