# Player GameObject

### 2D Sprite as Player

To create a player character, we need to create a gameObject =&gt;  2D Sprite. Then, we can set the sprite-Renderer component to refer to an image that we've added to our project assets folder.  To import an image as a 2D sprite, you select the image in the assets panel and set the component: texture-type set to \*Sprite\(2D - UI\).

### Moving GameObjects: RigidBody2D

Then we need to attach a RigidBody2D component to our Girl-player gameObject.  The [Unity Manual](http://docs.unity3d.com/ScriptReference/Rigidbody2D.html) explains that we need to add a RigidBody2D component so that we can use the Unity Physics Engine to move our Girl-player in the scene.  We can update the player's RigidBody.velocity or add a force to the RigidBody2D each frame in response to the user key-presses, to cause the gameObject to move in a realistic way.


### Custom Sorting Layers

We need to define sorting layers for our sprites, this will allow us to define the ordering that sprites will be rendered in, where items with a higher order in the sorting layer list being rendered prior to sprites from a sorting layer lower in the list. Sprites rendered first appear behind sprites that are rendered later in the order. First we need to select the layer dropdown tab from the top of the inspector panel and then we choose to add new layers.  We select to add new sorting layers, we create a Background and a Foreground sorting layer.  The ordering of the layers in this list defines the ordering of layers in our scene. Then for each GameObject that we assign with _Sorting Layer Background_ those objects will be behind the GameObjects that have been assigned to the Foreground Layer.  We can define additional layers, and we can change the layer ordering dynamically using C\# scripts.  \(We don't do that in this project\)  
In addition, the Z transform value for an object can also determine sprite visibility layering for objects on the same sorting layer. The Main Camera has a transform position vector of \(0,0,-10\) by default.  This means that to move objects closer to the camera, can give them a Z transform value that is between 0 and -10.

### RigidBody2D

In the PlayerMove code below, we write code to allow the user to move the player using various input methods. We're using the UnityEngine Input default mappings so we want to provide access to move the player along the horizontal and vertical axis.  We need to use a RigidBody2D  component because it allows us to use UnityEngine physics engine to create realistic character movement.

[Unity RigidBody Documentation](http://docs.unity3d.com/Manual/class-Rigidbody.html)  
 Rigidbodies enable your GameObjects to act under the control of physics. The Rigidbody can receive forces and torque to make your objects move in a realistic way.  
[Rigidbody 2D](http://docs.unity3d.com/Manual/class-Rigidbody2D.html), the difference that in 2D, objects can only move in the XY plane and can only rotate on an axis perpendicular to that plane.  Remember to set gravity to 0 if you don't want it to impact your objects.

### 2D-Colliders

[Collider interactions](http://docs.unity3d.com/Manual/CollidersOverview.html)  Colliders interact with each other differently depending on how the Collider and Rigidbody components are configured. Collider Components define the shape of an object for the purposes of physical collisions. A collider, which is invisible, does not have to be the exact same shape as the object’s mesh. A GameObject that has a Collider but no RigidBody component will act like an immovable object that other objects can run into, the collider gives the gameObject a physical presence in the scene.  When `isTrigger` is selected, then the collider does not show physics-based collision behavior, instead, an event is generated that can be used to execute related actions or behaviors.

### Constrain Player Movement using Box Collider to create a Floor

If we want to constrain the player, to keep her on the screen, we can create an empty game object and attach a collider2D component to that gameObject.  We can use this to create a floor in our scene, this will allow us to have a non-zero value for gravity on our player's Rigidbody2D component.  We can also create a leftBorder as an empty gameObject, again, add a  BoxCollider2D component.  If we don't set this gameObject to be a trigger, then it will act as a physical barrier to our playerGame object so it can't leave the camera view area.

## PlayerController.cs Code:  Attached to the Player Sprite
###Version 1 - Does not include animation

###PickUp objects must have Tags: 
PickUp items must have Tags set to match logic in PlayerController.cs:  **Collectible, Hazard**

The code in the PlayerController.cs file is responsible for checking for user-input, determining if the input should cause the player-gameObject to move, and for determining actions or game-logic to be executed if the player-gameObject collides with other gameObjects.

###PlayerController.cs - No Animation

```java
using UnityEngine;
using System.Collections;

public class PlayerController : MonoBehaviour {

        private Rigidbody2D rb2D;
        public float forceX;
        private bool facingRight; 
            
        void Start ()
        {
            rb2D = GetComponent<Rigidbody2D> ();
            facingRight = true;
            forceX = 90f; //if initialized here can't be overridden in Inspector
        }

        void FixedUpdate ()
        {
            float inputX = Input.GetAxis ("Horizontal");
            bool isWalking = Mathf.Abs (inputX) > 0;

            if (isWalking) {
                if (inputX > 0 && !facingRight) {
                    Flip ();
                    Debug.Log ("flip right");
                }
                if (inputX < 0 && facingRight) {
                    Flip ();
                    Debug.Log ("flip left");
                }
                rb2D.velocity = new Vector2 (0, 0);  // reset velocity to 0
                rb2D.AddForce (new Vector2 (forceX * inputX, 0));
            } 

        }

       private void Flip ()
        {
            facingRight = !facingRight;
            Vector3 theScale = transform.localScale;
            theScale.x *= -1;
            transform.localScale = theScale;
        }

        ////This is the EVENT that DRIVES the MiniGame, Player colliding with Pickup Objects
        void OnTriggerEnter2D (Collider2D hitObject)
        {
            if (hitObject.CompareTag ("Collectible")) {
                Debug.Log ("Hit Collectible");
                PickUp item = hitObject.GetComponent<PickUp> ();  
                //will be updated later so that full item is passed to GameData's Add method so items can be added to inventory             
                 GameData.instanceRef.Add(item.value );
                 Destroy (hitObject.gameObject);
             }
            else if(hitObject.CompareTag("Hazard")){
                Debug.Log ("Hit Hazard");
                PickUp item = hitObject.GetComponent<PickUp> ();  
                GameData.instanceRef.TakeDamage( item.value);
                Destroy (hitObject.gameObject);
            }
            else {
                Debug.Log ("collided with some other object");
            }
        } //end OnTriggerEnter2D
        
    }  // end class
```




