#Spawner, Integrated with PickUp Unity Event

###Pickup With UnityEvent - onDied
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
##Spawner with PickUp onDied Event


```java
using UnityEngine;
using System.Collections;
using System.Collections.Generic;

public class Spawner : MonoBehaviour
{

    // The prefab we will spawn
    [Header("Set in Inspector")]
    public GameObject prefabBad;
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
                Invoke("SpawnPrefab", Random.Range(pauseTime, pauseTime * 2.0f));
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

