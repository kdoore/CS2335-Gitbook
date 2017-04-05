# Player GameObject

### 2D Sprite as Player

To create a player character, we need to create a gameObject =&gt;  2D Sprite. Then, we can set the sprite-Renderer component to refer to an image that we've added to our project assets folder.  To import an image as a 2D sprite, you select the image in the assets panel and set the component: texture-type set to \*Sprite\(2D - UI\).

Below are some example open-source game sprites that you can download to use in your proejct, these sprites are from:  [Daniel Cook's Planet Cute](http://www.lostgarden.com/2007/05/dancs-miraculously-flexible-game.html)

![](girl1.png)![](healthheart.png)  
![](star.png)

### Moving GameObjects: RigidBody2D

Then we need to attach a RigidBody2D component to our Girl-player gameObject.  The [Unity Manual](http://docs.unity3d.com/ScriptReference/Rigidbody2D.html) explains that we need to add a RigidBody2D component so that we can use the Unity Physics Engine to move our Girl-player in the scene.  We can update the player's RigidBody.velocity or add a force to the RigidBody2D each frame in response to the user key-presses, to cause the gameObject to move in a realistic way.

### Canvas Mode:  Screen-Space Camera

Since we are using sprites to create a game in this scene, we'll need to change our camera settings to allow us to also use UI-objects that will be on a Canvas GameObject. Add a UI-Text GameObject to the scene.  If you double click on your text, you'll see that the scaling for the canvas does not match the scaling for the sprites we've used to create the scene.

### Custom Sorting Layers

We need to define sorting layers for our sprites, this will allow us to define layers that vary in Z depth. First we need to select the layer dropdown tab from the top of the inspector panel and then we choose to add new layers.  We select to add new sorting layers, we create a Background and a Foreground sorting layer.  The ordering of the layers in this list defines the ordering of layers in our scene. Then for each GameObject that we assign with _Sorting Layer Background_ those objects will be behind the GameObjects that have been assigned to the Foreground Layer.  We can define additional layers, and we can change the layer ordering dynamically using C\# scripts.  \(We don't do that in this project\)  
In addition, the Z transform value for an object can also determine visibility.  The Main Camera has a transform position vector of \(0,0,-10\) by default.  This means that to move objects closer to the camera, can give them a Z transform value that is between 0 and -10.

### RigidBody2D

In the PlayerMove code below, we write code to allow the user to move the player using various input methods. We're using the UnityEngine Input default mappings so we want to provide access to move the player along the horizontal and vertical axis.  We need to use a RigidBody2D  component because it allows us to use UnityEngine physics engine to create realistic character movement.

[Unity RigidBody Documentation](http://docs.unity3d.com/Manual/class-Rigidbody.html)  
 Rigidbodies enable your GameObjects to act under the control of physics. The Rigidbody can receive forces and torque to make your objects move in a realistic way.  
[Rigidbody 2D](http://docs.unity3d.com/Manual/class-Rigidbody2D.html), the difference that in 2D, objects can only move in the XY plane and can only rotate on an axis perpendicular to that plane.  Remember to set gravity to 0 if you don't want it to impact your objects.

### Colliders

[Collider interactions](http://docs.unity3d.com/Manual/CollidersOverview.html)  Colliders interact with each other differently depending on how the collider and Rigidbody components are configured. The three important configurations are the Static Collider \(ie, no Rigidbody is attached at all\), the Rigidbody Collider and the Kinematic Rigidbody Collider.  Collider Components define the shape of an object for the purposes of physical collisions. A collider, which is invisible, does not have to be the exact same shape as the object’s mesh. A GameObject that has a Collider but no RigidBody component will act like an immovable object that other objects can run into, the collider gives the gameObject a physical presence in the scene.  When isTrigger is selected, then the collider does not show collision behavior, instead, an event is generated that can be used to execute related actions or behaviors.

### PlayerController.cs Code:  Attached to the Player Sprite

The code in the PlayerController.cs file is responsible for checking for user-input, determining if the input should cause the player-gameObject to move, and for determining actions or game-logic to be executed if the player-gameObject collides with other gameObjects.

PlayerController.cs

```
using UnityEngine;
using System.Collections;

public class PlayerControllerSimple : MonoBehaviour {


        private Transform myTransform;
        private Rigidbody2D myRBody2D;
        public float forceX;
        private bool facingRight; 


        void Awake ()
        {
            myTransform = GetComponent<Transform> ();
            myRBody2D = GetComponent<Rigidbody2D> ();

        }

        void Start ()
        {
            facingRight = true;
            forceX = 50f;
        }

        void FixedUpdate ()
        {
            float inputX = Input.GetAxis ("Horizontal");
            bool isWalking = Mathf.Abs (inputX) > 0;

            if (isWalking) {
                if (inputX > 0 && !facingRight) {
                    flip ();
                    Debug.Log ("flip right");
                }
                if (inputX < 0 && facingRight) {
                    flip ();
                    Debug.Log ("flip left");
                }
                myRBody2D.velocity = new Vector2 (0, 0);  // reset velocity to 0
                myRBody2D.AddForce (new Vector2 (forceX * inputX, 0));
            } 

        }

        void flip ()
        {
            facingRight = !facingRight;
            Vector3 theScale = myTransform.localScale;
            theScale.x *= -1;
            myTransform.localScale = theScale;
        }

        ////This is the EVENT that DRIVES the MiniGame, Player colliding with Pickup Objects
        void OnTriggerEnter2D (Collider2D hitObject)
        {
            Debug.Log ("Entered Trigger");
            if (hitObject.CompareTag ("PickUp")) {
                Debug.Log ("Hit Pickup");
                PickUp item = hitObject.GetComponent<PickUp> ();
                GameData.instanceRef.Add (item);

            Destroy (hitObject.gameObject);
            } else {
                Debug.Log ("collided with some other object");
            }
        }
    }  // end class
```

### Constrain Player Movement using Box Collider to create a Floor

If we want to constrain the player, to keep her on the screen, we can create an empty game object and attach a collider2D component to that gameObject.  We can use this to create a floor in our scene, this will allow us to have a non-zero value for gravity on our player's Rigidbody2D component.  We can also create a leftBorder as an empty gameObject, again, add a  BoxCollider2D component.  If we don't set this gameObject to be a trigger, then it will act as a physical barrier to our playerGame object so it can't leave the camera view area.

### 



