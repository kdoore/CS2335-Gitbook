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
//Code Updated 4/12/18 2:00 pm
using System.Collections;
using System.Collections.Generic;
using UnityEngine;
using UnityEngine.UI;
using UnityEngine.Events;
using UnityEngine.SceneManagement;

public class Spawner : MonoBehaviour {

    // The prefab we will spawn
    [Header("Set in Inspector")]
    //public GameObject goodPrefab, badPrefab;
    public List<GameObject> prefabsGood;

    [Header("Set in Inspector")]
    public List<GameObject> prefabsBad;

    // Use this for initialization
    private int pauseTime = 2;//wait 2 sec before starting
    public int numToSpawn = 3;
    public Vector3 PlaceToSpawn;
    public float TimeBetweenSpawns= 1.5f;
    public float chanceToSpawnBad = 0.11f;
    public float chanceToSpawnGood = 0.5f;
    public bool activeSpawning = false;
   

    // Use this for initialization


    void Start()
    {
        StartSpawning(); //call in LevelManager
        Invoke("Spawn", TimeBetweenSpawns);
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


        float rand = Random.value;
        GameObject item;
        PickUp spawnedItem;
        //check to make sure there are objects to spawn
        if (rand < chanceToSpawnBad && prefabsBad.Count > 0)
        {
            int randIndex = Random.Range(0, prefabsBad.Count);
            item = Instantiate(prefabsBad[randIndex], position, transform.rotation);
            spawnedItem = item.GetComponent<PickUp>();
            spawnedItem.OnDied.AddListener(SpawnNewOne);
        }
        else if (prefabsGood.Count > 0)
        { //pick a good item from the array using random index 
            int randIndex = Random.Range(0, prefabsGood.Count);
            item = Instantiate(prefabsGood[randIndex], position, transform.rotation);
            spawnedItem = item.GetComponent<PickUp>();
            spawnedItem.OnDied.AddListener(SpawnNewOne);
        }
        //get the PickUp component so we can 
        //register as a listener for the OnDied event 
        //for the Spawned object
          //Call SpawnPrefab again when this spawnedItem dies

    }

    public void SpawnNewOne()
    {
        if (activeSpawning)
        {
            SpawnPrefab(); //this version gives no delay 
                           // Invoke("SpawnPrefab", Random.Range(pauseTime, pauseTime * 2.0f)); //this version gives a delay
        }
        Debug.Log("Spawned new prefab");
    }

    public void DestroyAllSpawnedObjects()
    {
        GameObject[] goodItems = GameObject.FindGameObjectsWithTag("Collectible");
        foreach (var item in goodItems)
        {
            Destroy(item);
        }
        GameObject[] badItems = GameObject.FindGameObjectsWithTag("Hazard");
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

