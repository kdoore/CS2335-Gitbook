# PickUp Items

Throughout our game we will want to have game items for the player to interact with.  Often these are considered Pick-up items.  So, we'll want to create a base-class that represents this Pick-up type and create child classes to extend for specialized pick-up items.  Some of our pick-up objects will need the ability to generate events that can notify other objects when the pick-up item has died.  

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
    public int damage;

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

```
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
            OnDied.Invoke(); // notify the spawner
        }

        Destroy (gameObject);
        //Remove all non - persisent(ie created from script) listeners from the event.
        OnDied.RemoveAllListeners(); //unregister the spawner, so the event-connection is removed 
                                    //and this guy can die in peace.
    }

}
  //end class
```

### SelfDestructPickup: Child Class of PickUp

```
using UnityEngine;
using System.Collections;

public class SelfDestructPickup : PickUp {

    // Life variables
    private float minLifeTime;
    private float maxLifeTime;

   
    void Start () {
        // Automatic destroy after random time by calling base class died method
        minLifeTime = 100.0f;
        maxLifeTime = 400.0f;
        type = PickupType.crystal;
         Invoke ("DestroyMe", Random.Range(minLifeTime, maxLifeTime));  //
    }
}
```



