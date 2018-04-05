#PickUp - Simple

The code below is for the most basic type of PickUp.
You will need to change the PickupType enums, add new enums to match your game scenario.  Below this code section is code for a PickUp with custom events, which can be used with a spawner.

The PickUp gameObject should be a 2D sprite with the following details:
	- Collider2D, with isTrigger selected
	- Tag:  Collectible or Hazard
	- PickUp script added to the gameObject
	- Populate in the inspector

```java
using UnityEngine;
using System.Collections;
using System;

public enum PickupType
{
	crystal,
	star,
	rock,
        gem,
        bug
};

public class PickUp : MonoBehaviour
{
	[Header("Set in Inspector")]
	public PickupType type;
	
	[Header("Set in Inspector")]
	public int value;

	public void DestroyMe () //executed by Animation-Trigger
	{
		Debug.Log ("Item Destroy Me");
		Destroy (gameObject);
	}

}//end class
```


# PickUp - With UnityEvent
The code below can be used to create a PickUp GameObject / Prefab, that can notify a spawner when it is going to be destroyed.  This can enable the spawner to spawn new objects when a PickUp is destroyed.


```java
using UnityEngine;
using UnityEngine.Events;  //ADD THIS
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
    
    public UnityEvent OnDied;  //custom Event
	
	[Header("Set in Inspector")]
	public PickupType type;
	
	[Header("Set in Inspector")]
	public int value;

    public void Start()
    {
        if(OnDied == null){ //initailize the event, call the constructor
            OnDied = new UnityEvent();
        }
    }
    public void DestroyMe () //execute to manage destroying the object and invokeing the event.
	{
		Debug.Log ("Item Destroy Me");
        if (OnDied != null)  //someone is listening (spawner)
        { //initailize the event, call the constructor
            OnDied.Invoke(); // broadcast event: (notify the spawner)
            OnDied.RemoveAllListeners(); //unregister the spawner, so no traces of the connection remain.

        }

        Destroy (gameObject);
    }

}
  //end class
```




