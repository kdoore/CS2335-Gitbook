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
//Code Updated 4/12/18 10:50 am
using UnityEngine;
using System.Collections;
using System.Collections.Generic;

public class Spawner : MonoBehaviour
{

    // The prefab we will spawn
    [Header("Set in Inspector")]
    public GameObject prefabBad;

    [Header("Set in Inspector")]
    public List<GameObject> prefabsGood;

    // Use this for initialization

    private int pauseTime = 2;//wait 2 sec before starting
    public int numToSpawn = 3;  //total number in scene at anytime
    public float chanceToSpawnBad = .1f; //10% chance of bad, adjust as needed

    //Adjust range values as needed
    public float xRange = 8.0f; //left - right borders
    public float yRangeTop = -2.0f;  //modify to fit your game
    public float yRangeBottom = -3.5f;  //modify to fit your game

    public bool activeSpawning = false;

    void Start()
    {
        // StartSpawning(); //call in LevelManager
        activeSpawning = true;
    }

    public void StartSpawning()
    {
        for (int i = 0; i < numToSpawn; i++)
        {
            Invoke("SpawnPrefab", Random.Range(pauseTime, pauseTime * 2.0f));
        }

    }

    //Randomly spawns a prefab
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

        }
        else
        { //pick a good item from the array using random index 
            int randIndex = Random.Range(0, prefabsGood.Count);
            item = Instantiate(prefabsGood[randIndex], position, transform.rotation);

        }
        //get the PickUp component so we can 
        //register as a listener for the OnDied event 
        //for the Spawned object
        spawnedItem = item.GetComponent<PickUp>();
        spawnedItem.onDied.AddListener(SpawnNewOne);  //Call SpawnPrefab again when this spawnedItem dies
    }

    public void SpawnNewOne()
    {
        if (activeSpawning) //only spawn if activeSpawning is true
        {
            SpawnPrefab(); //this version gives no delay 
                           // Invoke("SpawnPrefab", Random.Range(pauseTime, pauseTime * 2.0f)); //this version gives a delay
        }
        Debug.Log("Spawned new prefab");
    }

    ///This method can be called from any other script using the Spawner object, to destroy all spawned objects with Tags as shown.

    public void DestroyAllSpawnedObjects()
    {
        GameObject[] goodItems = GameObject.FindGameObjectsWithTag("Collectible");
        Debug.Log("Destroyed good items " + goodItems.Length);
        foreach (var item in goodItems)
        {
            Destroy(item);   //destroy without calling item.DestroyMe()
        }
        GameObject[] badItems = GameObject.FindGameObjectsWithTag("Hazard");
        Debug.Log("Destroyed bad items " + badItems.Length);
        foreach (var item in badItems)
        {
            Destroy(item);
        }
    }

}//end of class

```
###LevelManager Code to Stop Spawning
The code below shows how to stop spawning for the spawner object, by setting activeSpawning to false. Calling the DestroyAllSpawnedObjects( ) will destroy all pickup items remaining objects in the scene. 


```java
void loadLevel2()
    {
        ///STOP LEVEL 1 SPAWNER
        spawner.activeSpawning = false;
        spawner.DestroyAllSpawnedObjects();

        //start new spawner
        spawner2.StartSpawning();

        levelText.text = "Level 2";
    }

```

