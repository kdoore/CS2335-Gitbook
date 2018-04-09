# Event Driven Spawner

In order to dynamically spawn game objects in a scene, we'll need to first create a Prefab of a gameObject.  A prefab is an asset that we can include in any scene, we can consider that it is a preconfigured gameObject, which can include any valid components such as script and animator as pre-configured components which are instantiated when an instance of a Prefab is instantiated in a scene.

### PickUp with UnityEvent: OnDied

In our mini-game,  We want the PickUp to generate an custom event when it's destroyed, so it can notify the spawner to spawn a new PickUp.


```java
using UnityEngine;
using UnityEngine.Events; //ADD THIS
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
            OnDied.RemoveAllListeners(); //unregister the spawner, so the event-connection is removed 
        }

        Destroy (gameObject);
        //Remove all non - persisent(ie created from script) listeners from the event.
       
    }

}
  //end class
```


###Self-Destructive PickUp
This code creates a PickUp that destroys itself, within some random time, after it was created. 

```java
using UnityEngine;
using UnityEngine.Events;  //ADD THIS
using System.Collections;

public class SelfDestructPickup: PickUp {

    // Life variables
    private float minLifeTime;
    private float maxLifeTime;

    void Start () {
        // Self-destruct after random time by calling base class died method
        minLifeTime = 100.0f;
        maxLifeTime = 400.0f;
        
        Invoke ("DestroyMe", Random.Range(minLifeTime, maxLifeTime));  //
    }
}
```

### Spawner - Listen for notification of dying PickUps

We need to create a custom scritp that can spawn our game objects:  
This script needs to be attached to an empty gameObject in our scene.  The spawner will 



```java
using UnityEngine;
using System.Collections;

public class Spawner : MonoBehaviour {

  // The prefab that we're going to spawn 
    public PickUp prefab;  //use baseclass type
    
    private int pauseTime;

    // Explicitly Create the first crystals in unity start
    void Start () {
        pauseTime = 1;
        for (int i = 0; i < prefabs.Count; i++){
            Invoke("SpawnPrefab", Random.Range(pauseTime+1, pauseTime+5)); 
        }
    }

    // Helper function to spawn prefabs and to register as a subscriber
    // to the crystal OnDied event notification
    public void SpawnPrefab (){ //this is the actual SpawnEvent

        //select position to spawn based on spawner position
        Vector3 pos = transform.position;
        pos.x = Random.Range(-6.0f, 6.0f);  //figure these range values based on scene geometry - move temp prefab to min, max positions
        pos.y = Random.Range (-4.8f, -4.2f);

        // Spawn the Pickup and assign to a temp object reference
        PickUp newPickup = (PickUp)Instantiate(prefab,pos,transform.rotation);

        // Subscribe so the other class handles notification automatically
        ///the spawn object has it's OnPickUpDied function added to the list of subscribers

        newPickup.onDied += ReSpawnItem; //when this crystal dies, please notify this spawn class

    }


    // OnPickUpDied is an EventHandler Function that will be called automatically when the pickup object instance dies
    // This allows it to spawn a new prefab instance, and then 
   
    public void ReSpawnItem (){

        // Spawn a new crystal 
        Invoke("SpawnPrefab", Random.Range(pauseTime, pauseTime+3));
    }

}
```



