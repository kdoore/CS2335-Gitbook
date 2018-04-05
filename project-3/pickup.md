#PickUp - Simple

The code below is for the most basic type of PickUp.
You will need to change the PickupType enums, add new enums to match your game scenario.  Below this code section is code for a PickUp with custom events, which can be used with a spawner.

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
	
	public PickupType type;
	public int value;

	public void DestroyMe () //executed by Animation-Trigger
	{
		Debug.Log ("Item Destroy Me");
		Destroy (gameObject);
	}

}//end class
```


# PickUp - With UnityEvent
The 


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




