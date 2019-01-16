#Getting Started - Project 1

After downloading and installing the Unity Editor, Open the Unity application and create a new **2D** project.  

###Download Free Game Art Assets:
    
**GameArt 2D Free Assets**  
[Winter Tile Set](https://www.gameart2d.com/winter-platformer-game-tileset.html)

[Cat and Dog Sprites](https://www.gameart2d.com/cat-and-dog-free-sprites.html)  
    
**Planet Cute** [Planet Cute Zipfile ](https://utdallas.box.com/v/planet-cute-zipfile) - from: http://www.lostgarden.com/2007/05/dancs-miraculously-flexible-game.html


###Add Sprites to Unity Project Assets 
1. Create a new folder: _**textures**_ in the Project / Assets windows by right-clicking and selecting new folder 
2. Add .png files to your Unity project by dragging each file into the textures folder in your project / assets window. 

###2D Sprite GameObject:
Select GameObject in the top menu, select: GameObject> 2D Object> Sprite, this should add a new item in the **Hierarchy panel**.  In the **Inspector panel**, select a sprite image (from the assets tab) to associate with this new 2D object by selecting the small circle icon to the right of the Sprite Rendererer Component's **Sprite** field. 

###Collider2D Components
Gives sprites a collision boundry - allows for collision interactions with other objects
    - **IsTrigger** - when checking this checkbox: then this collider **will not display** collision interaction behavior, but it will cause the **OnTriggerEnter2D** event to be exectued. This is often used for sensing movement into zones, or for for objects that will be destroyed.   

###Create and Configure 2D Sprite GameObjects:
1.  Background Image - scale to fill the camera's viewport.
2. Player gameObject  
    - Add Physics2D Capsule Collider Component
    
3. Several _PickUp_ objects: (objects for the player to interact with)
    - Add a Physics2D Collider to each object
    
4. Floor - Create an empty GameObject, attach a  BoxCollider2D, edit the collider so it is a wide rectangle, move toward the bottom of the screen.

5. PlayerController Script
    


```java

using System.Collections;
using System.Collections.Generic;
using UnityEngine;

public class PlayerController : MonoBehaviour
{
    private Rigidbody2D myRBody2D;
    public float forceX = 100f;
   
    void Awake()
    {
       myRBody2D = GetComponent<Rigidbody2D>();
    }
    
    
    void FixedUpdate()
    {
        float inputX = Input.GetAxis("Horizontal");
        bool isWalking = Mathf.Abs(inputX) > 0;

        if (isWalking)
        {
            myRBody2D.velocity = new Vector2(0, 0);  // reset velocity to 0
            myRBody2D.AddForce(new Vector2(forceX * inputX, 0));
        }
    }

    ////This is the EVENT that DRIVES the MiniGame, Player colliding with Pickup Objects
    void OnTriggerEnter2D(Collider2D hitObject)
    {
        Debug.Log("Entered Trigger");

        if (hitObject.CompareTag("Collectible"))
        {
            Debug.Log("Hit Collectible");
            Destroy(hitObject.gameObject);
        }
        else if(hitObject.CompareTag("Hazard"))
        {
            Debug.Log("collided with Hazard");
            Destroy(hitObject.gameObject);
        }
    }
}  // end class
    

```

    

