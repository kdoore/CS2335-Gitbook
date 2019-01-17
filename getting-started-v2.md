# Getting Started - Project 1

#### Create a new 2D project in Unity

After downloading and installing the Unity Editor, Open the Unity application and create a new **2D** project.

#### Download Free Game Art Assets:

**GameArt 2D Free Assets**  
[Winter Tile Set](https://www.gameart2d.com/winter-platformer-game-tileset.html)

[Cat and Dog Sprites](https://www.gameart2d.com/cat-and-dog-free-sprites.html)

**Planet Cute** [Planet Cute Zipfile ](https://utdallas.box.com/v/planet-cute-zipfile) - from: [http://www.lostgarden.com/2007/05/dancs-miraculously-flexible-game.html](http://www.lostgarden.com/2007/05/dancs-miraculously-flexible-game.html)

Images Below from Planet Cute: Star, CatGirl, EnemyBug:

![](/star.png)![](/Character Cat Girl.png)![](/assets/Enemy Bug.png)

#### Add Sprites to Unity Project Assets

1. Create a new folder: _**textures**_ in the Project / Assets windows by right-clicking and selecting new folder 
2. Add .png files to your Unity project by dragging each file into the textures folder in your project / assets window. 
3. When you select an image in your assets folder, the Inspector should show that the **texture-type is Sprite \(2D and UI\)**

#### 2D Sprite GameObject:

Select GameObject in the top menu, select: GameObject&gt; 2D Object&gt; Sprite, this should add a new item in the **Hierarchy panel**.  In the **Inspector panel**, select a sprite image \(from the assets tab\) to associate with this new 2D object by selecting the small circle icon to the right of the Sprite Rendererer Component's **Sprite** field.

#### Collider2D Components

Gives sprites a collision boundary - allows for collision interactions with other objects, objects with Collider2D have a physical boundary

* **IsTrigger** - when checking this checkbox: then this collider **will not display** collision interaction behavior, but it will cause the **OnTriggerEnter2D** event to be exectued. This is often used for sensing movement into zones, or for for objects that will be destroyed.   

### Create and Configure 2D Sprite GameObjects:

1. Background Image - scale to fill the camera's viewport. 
2. **Player** gameObject - 2D Sprite GameObject

   * Add **Physics2D &gt; Collider2D** Components - select 1 or more Colliders to fit your gameObject
   * Add **Physics2D &gt; RigidBody2D** Component - this is required for objects that will have **movement**, physics forces should be used to give movement to gameObjects.

3. Several** **2D Sprite Game Objects: \(objects for the player to interact with - PickUp \)

   * Add one or more **Physics2D &gt; Collider2D** components to create a collision-boundry for each object
   * Select **IsTrigger** checkbox for these collider components.

4. **Floor** - Create an empty GameObject, attach a  BoxCollider2D, edit the collider so it is a wide rectangle, move toward the bottom of the screen.

5. Create C\# Script:  **PlayerController** - Simple Version \(script provided below\)

6. ### Player Controller Script - version 1

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
            myRBody2D.AddForce(new Vector2(forceX * inputX, 0)); update x component velocity by adding a force, nothing happens to y velocity
        }
    }

    ////This is the EVENT that DRIVES the MiniGame, Player colliding with Pickup Objects
    //Custom Tags: Collectible and Hazard must be created in Unity and added to the PickUp objects.
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

### GameData - version1

```java
using System.Collections;
using System.Collections.Generic;
using UnityEngine;

public class GameData : MonoBehaviour {

    public int score;
    public int health;
    // Use this for initialization
    void Start () {
        score = 0;
        health = 100;
    }

    void Add( int points ){
        score += points;
        Debug.Log("Score updated " + score);
    }

    void TakeDamage( int points){
        health -= points;
        Debug.Log("Health updated " + health);
    }
}
```

### PickUp - version1

```java
using System.Collections;
using System.Collections.Generic;
using UnityEngine;

public class PickUp: MonoBehaviour {

    public int value;
    public string type;
    
}
```



