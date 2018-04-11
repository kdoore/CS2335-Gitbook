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
using System.Collections.Generic;

public class Spawner : MonoBehaviour
{

    // The prefab we will spawn
    [Header("Set in Inspector")] ///ONLY ONE BAD ONE USED HERE
    public GameObject prefabBad;
    
     [Header("Set in Inspector")] //LIST OF GOOD ONES
    public List<GameObject> prefabsGood;

    // Use this for initialization

    private int pauseTime=2;//wait 2 sec before starting
    public int numToSpawn=3;
    public float chanceToSpawnBad = .01f;
    public float xRange = 8.0f;
    public float yRangeTop = -2.0f;
    public float yRangeBottom = -3.5f;

    void Start()
    {
       StartSpawning (); //call in LevelManager
    }

    public void StartSpawning()
    {

       for (int i = 0; i < numToSpawn; i++)
            {
                Invoke("SpawnPrefab", Random.Range(pauseTime, pauseTime * 2.0f *i));  ///for delay
            }

    }

    
    public void SpawnPrefab()
    {
        Vector3 position = transform.localPosition;
        position.x = Random.Range(-xRange, xRange);
        position.y = Random.Range(yRangeBottom, yRangeTop);

        float rand = Random.value;
        GameObject item;
        PickUp spawnedItem;
       
    if (rand < chanceToSpawnBad)
        {
            item = Instantiate(prefabBad, position, transform.rotation);

        }else{ //pick a good item from the array using random index 
            int randIndex = Random.Range(0, prefabsGood.Count);
            item = Instantiate(prefabsGood[randIndex], position, transform.rotation);

        } 
        //get the PickUp component so we can 
        //register as a listener for the OnDied event 
        //for the Spawned object
        spawnedItem = item.GetComponent<PickUp>();
        spawnedItem.onDied.AddListener(SpawnNewOne);  //Call SpawnPrefab again when this spawnedItem dies
    }

    public void SpawnNewOne(){
        Invoke("SpawnPrefab", Random.Range(pauseTime, pauseTime * 2.0f));
        Debug.Log("Spawned new prefab");
    }
}
```

##Code Change is required in OnTriggerEnter PlayerController.cs

```java
void OnTriggerEnter2D (Collider2D hitObject)
	{
        
		if (hitObject.CompareTag ("Collectible")) {
			
            PickUp item = hitObject.GetComponent<PickUp>();
            GameData.instanceRef.Add(item);

            item.DestroyMe();  //ADD THIS CODE
            //Destroy(hitObject.gameObject); //REMOVE THIS CODE
            Debug.Log("Hit Collectible");
		}
		if (hitObject.CompareTag ("Hazard")) {
            anim.SetInteger("HeroState", (int)heroState.dead);

            PickUp item = hitObject.GetComponent<PickUp>();
            GameData.instanceRef.TakeDamage(item.value);

            item.DestroyMe();  //ADD THIS CODE will invoke onDied event to spawn new prefab
            //Destroy(hitObject.gameObject); //REMOVE THIS CODE
            dead = true;
          
            Debug.Log("Hit Hazard");
		}
	}





```

