# PickUp Items - Prefab GameObjects

Throughout our game we will want to have game items for the player to interact with.  Often these are considered Pick-up items.

###Create / Configure PickUp PreFab GameObject
In Unity, we'll want to create and configure gameObjects that will have this script attached to give desired behavior.
You are required to have 4 types of PickUp objects in your game, 3 must have tag: Collectible, 1 can have tag: Hazard.
[See additional information - Create 2D Sprite Prefab: Rock](/create-2d-sprite-prefab-rock.md)

    - Create a 2D Sprite GameObject
    - Add a Collider2D to the GameObject, configure collider shape
    - Configure the Collider2D to be isTrigger == true
    - Select a tag for the gameObject:  "Collectible", "Hazard", other?
    - Add Rigidbody2D if the gameObject will move
    - Optional add additional Collider2D inside trigger Collider2D, to keep this gameObject from falling through the floor's Collider2D.
    - Add PickUp script
        - Select type from dropdown in inspector
        - Set value, set a positive value
    - Drag into Project Assets Resource Folder to create as a prefab

### Simple PickUp Class
###Version 1
```
using UnityEngine;
using System.Collections;
using System;

public class PickUp : MonoBehaviour
{
    public int value;

}
//end class
```

### Simple PickUp Class
###Version 2

```
using UnityEngine;
using System.Collections;
using System;

public enum PickupType
{
    crystal,
    animatedCrystal,
    mushroom,
    rock
};

public class PickUp : MonoBehaviour
{

    public PickupType type;
    public int value;

  
}
//end class
```
