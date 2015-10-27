# Inventory

Inventory Code: Unity 5x Cookbook by Matt Smith PacktPub.  

Sprites are from [LostGarden](LostGarden.com): Daniel Cook.[Small World Graphics](http://www.lostgarden.com/search/label/free%20game%20graphics)

[Zip File with Sprite Assets:](https://utdallas.box.com/s/f8q6fgq67v7r097zr49tk0lzrxrewj63)

For the inventory, we're going to create a player that can interact with other gameObjects, as a means to interact with items in their inventory.  

The [Unity Script Reference Manual](http://docs.unity3d.com/ScriptReference/) provides documentation for how to create game-like behaviors as interactions between GameObjects.

### 2D Sprite as Player 

We'll start by dragging an image, that's had it's import texture-type set to *Sprite(2D - UI)* to the Hierarchy Panel to create a 2D-sprite GameObject: *Girl-player* in a new scene.  Then we need to attach a RigidBody2D component to our Girl-player.  The [Unity Manual](http://docs.unity3d.com/ScriptReference/Rigidbody2D.html) explains that we need to add a RigidBody2D component so that we can use the Unity Physics Engine to move our Girl-player in the scene.  We'll update the player's RigidBody.velocity each frame in response to the user key-presses.

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