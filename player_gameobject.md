# Player GameObject


### 2D Sprite as Player 

We'll start by dragging an image, that's had it's import texture-type set to *Sprite(2D - UI)* to the Hierarchy Panel to create a 2D-sprite GameObject: *Girl-player* in a new scene.  Then we need to attach a RigidBody2D component to our Girl-player.  The [Unity Manual](http://docs.unity3d.com/ScriptReference/Rigidbody2D.html) explains that we need to add a RigidBody2D component so that we can use the Unity Physics Engine to move our Girl-player in the scene.  We'll update the player's RigidBody.velocity each frame in response to the user key-presses.

###Canvas Mode:  Screen-Space Camera
Since we are using sprites to create a game in this scene, we'll need to change our camera settings to allow us to also use UI-objects that will be on a Canvas GameObject. Add a UI-Text GameObject to the scene.  If you double click on your text, you'll see that the scaling for the canvas does not match the scaling for the sprites we've used to create the scene.

###Custom Sorting Layers

We need to define sorting layers for our sprites, this will allow us to define layers that vary in Z depth. First we need to select the layer dropdown tab from the top of the inspector panel and then we choose to add new layers.  We select to add new sorting layers, we create a Background and a Foreground sorting layer.  The ordering of the layers in this list defines the ordering of layers in our scene. Then for each GameObject that we assign with *Sorting Layer Background* those objects will be behind the GameObjects that have been assigned to the Foreground Layer.  We can define additional layers, and we can change the layer ordering dynamically using C# scripts.  (We don't do that in this project)

###Colliders

[Collider Documentation: ](http://docs.unity3d.com/Manual/CollidersOverview.html) Collider Components define the shape of an object for the purposes of physical collisions. A collider, which is invisible, need not be the exact same shape as the object’s mesh and in fact, a rough approximation is often more efficient and indistinguishable in gameplay.

[Collider interactions](http://docs.unity3d.com/Manual/CollidersOverview.html)

Colliders interact with each other differently depending on how their Rigidbody components are configured. The three important configurations are the Static Collider (ie, no Rigidbody is attached at all), the Rigidbody Collider and the Kinematic Rigidbody Collider.

###RigidBody2D

In the PlayerMove code below, we write code to allow the user to move the player using various input methods. We're using the UnityEngine Input default mappings so we want to provide access to move the player along the horizontal and vertical axis.  We need to use a RigidBody2D  component because it allows us to use UnityEngine physics engine to create realistic character movement.  

[Unity RigidBody Documentation](http://docs.unity3d.com/Manual/class-Rigidbody.html)
 Rigidbodies enable your GameObjects to act under the control of physics. The Rigidbody can receive forces and torque to make your objects move in a realistic way.
[Rigidbody 2D](http://docs.unity3d.com/Manual/class-Rigidbody2D.html), the difference that in 2D, objects can only move in the XY plane and can only rotate on an axis perpendicular to that plane.
   

###Player Move Code:  Attached to the Girl Sprite:

```
using UnityEngine;
using System.Collections;

public class PlayerMove : MonoBehaviour {
  public float speed = 10;
  private Rigidbody2D rigidBody2D;

  void Awake(){
    rigidBody2D = GetComponent<Rigidbody2D>();
  }

  void FixedUpdate(){
    float xMove = Input.GetAxis("Horizontal");
    float yMove = Input.GetAxis("Vertical");

    float xSpeed = xMove * speed;
    float ySpeed = yMove * speed;
    
    Vector2 newVelocity = new Vector2(xSpeed, ySpeed);
    
    rigidBody2D.velocity = newVelocity;
  }
}
```