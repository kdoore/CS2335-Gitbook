# Inventory

Inventory Code: Unity 5x Cookbook by Matt Smith PacktPub.  

Sprites are from [LostGarden](LostGarden.com): Daniel Cook.[Small World Graphics](http://www.lostgarden.com/search/label/free%20game%20graphics)

[Zip File with Sprite Assets:](https://utdallas.box.com/s/f8q6fgq67v7r097zr49tk0lzrxrewj63)


Player Move Code:  Attached to the Girl Sprite:

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