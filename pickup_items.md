# PickUp Items

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

### Variations - See code below

So, we'll want to create a base-class that represents this Pick-up type and create child classes to extend for specialized pick-up items.  [See SelfDestructPickup](https://kdoore.gitbooks.io/cs-2335/content/pickup_items.html#selfdestructpickup-child-class-of-pickup)

Some of our pick-up objects will need the ability to generate events that can notify other objects when the pick-up item has died.  [See PickUp with Events](https://kdoore.gitbooks.io/cs-2335/content/pickup_items.html#selfdestructpickup-child-class-of-pickup)

### Simple PickUp Class

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

    public void DestroyMe ()
    {
        Debug.Log ("Item Destroy Me");
        Destroy (gameObject);
    }

}
//end class
```

### Custom Events - Version of PickUp Class

In the example code below, we have introduced custom events: onDied, which uses the custom Delegate: OnDiedHandler\( \);  
This custom delegate and event can be used to have the PickUp object broadcast notification when the OnDied event happens, if any other gameObject components have subscribed as listeners to the OnDied event.

```java
using UnityEngine;
using UnityEngine.Events;
using System.Collections;
using System;

public enum PickupType
{
    crystal,
    star,
    animatedCrystal,
    rock
};

public class PickUp : MonoBehaviour
{
    //Add custom UnityEvent - Used to notify Spawner to spawn new object
    public UnityEvent OnDied; 

    public PickupType type;
    public int value;

    public void Start()
    {
        if(OnDied == null){ //initailize the event, call the constructor
            OnDied = new UnityEvent();
        }
    }
    public void DestroyMe () //execute to 
    {
        Debug.Log ("Item Destroy Me");
        if (OnDied != null)  //someone is listening (spawner)
        { //initailize the event, call the constructor
            OnDied.Invoke(); // broadcast event: (notify the spawner)
            OnDied.RemoveAllListeners(); //unregister the spawner, so the event-connection is removed 
//and this guy can die in peace.
        }

        Destroy (gameObject);
    }

}
  //end class
```

### SelfDestructPickup: Child Class of PickUp

The code below can be used to create a PickUp object that self-destructs after some amount of time. This child-class inherits events and methods from the base-class: PickUp.

```java
using UnityEngine;
using System.Collections;

public class SelfDestructPickup : PickUp {

    // Life variables
    private float minLifeTime;
    private float maxLifeTime;


    void Awake () {
        // Automatic destroy after random time by calling base class died method
        minLifeTime = 5.0f;
        maxLifeTime = 15.0f;

     //call's it's own DestroyMe method after a random delay
         Invoke ("DestroyMe", Random.Range(minLifeTime, maxLifeTime));  //
    }
}
```



