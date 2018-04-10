#PickUp - Simple

The code below is for the most basic type of PickUp.
You will need to change the PickupType enums, add new enums to match your game scenario.  Below this code section is code for a PickUp with custom events, which can be used with a spawner.

The PickUp gameObject should be a 2D sprite with the following details:
	- Collider2D, with `isTrigger` selected
	- Tag:  Collectible or Hazard
	- PickUp script added to the gameObject
	- Populate in the inspector: type, value
	- Sorting Layer: Forground
	- Layer: default is ok unless Physics behavior doesn't seem to be working.
	- Rigidbody2D if the pickUp will fall or move
	- Add an Additional Collider2D if the object will fall but must stop when hitting the floor

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
using UnityEngine.Events;
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
    public UnityEvent onDied;
	public PickupType type;
	public int value;

    void Start(){
        //initialize the UnityEvent by calling the constructor
        if(onDied == null){   
            onDied = new UnityEvent();
        }
    }

	public void DestroyMe () //executed by Animation-Trigger
	{
		//if there are any registered listeners, 
        //then broadcast / publish the event
        if(onDied != null){
            onDied.Invoke(); //tells the Spawner it has died so a new item can be spawned
            Debug.Log("Invoked event when PickUp died");
            onDied.RemoveAllListeners(); //remove spawner's registration/ listener connection
        }
        //Debug.Log("Item Destroy Me");
        Destroy(gameObject);
	}

}//end class

```




