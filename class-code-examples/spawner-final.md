#Spawner.cs - Final

This code includes updated code to: **DestroyAllSpawnedObjects**



```java
using UnityEngine;

public class Spawner : MonoBehaviour {

    [Header("Set in Inspector")]
    public GameObject goodPrefab, badPrefab;

    public int pauseTime = 2; //wait 2 seconds before spawning
    public int numToSpawn = 10;
    public float chanceToSpawnBad = .10f;
    public float xRange = 8.0f;
    public float yRangeTop = -2.0f;
    public float yRangeBottom = -3.5f;
    public bool activeSpawning = false;


    // Use this for initialization
    void Start() {
        //activeSpawning = true; ///remove later when we start spawning from another class
        //StartSpawning(); //remove later
    }

    public void StartSpawning()   //will be called from another class
    {
        for (int i = 0; i < numToSpawn; i++)
        {
            Invoke("SpawnPrefab", pauseTime * i * 2);
        }
    }

    /// <summary>
    /// Spawns one prefab:  good or bad with some probability of bad
    /// </summary>
    public void SpawnPrefab()
    {
        if (activeSpawning)
        {
            Vector3 position = transform.localPosition;
            position.x = Random.Range(-xRange, xRange);
            position.y = Random.Range(yRangeBottom, yRangeTop);
            float rand = Random.value; //returns value between 0.0 - 1.0 (property)
            GameObject prefab;
            if (rand < chanceToSpawnBad)
            {
                prefab = Instantiate(badPrefab, position, transform.rotation);    //instantiate bad
            }
            else  //instantiate good
            {
                prefab = Instantiate(goodPrefab, position, transform.rotation);
            }
            prefab.transform.SetParent(this.transform); //set Spawner as parent of prefabs in Hierarchy 
            Debug.Log("Spawned 1");
        }
    } //end SpawnPrefab


    public void DestroySpawnedObjects()
    {
        PickUp[] items = FindObjectsOfType<PickUp>();
        foreach (PickUp item in items    )
        {
            Destroy(item.gameObject);
        }

        Hazard[] hazards = FindObjectsOfType<Hazard>();
        foreach (Hazard hazard in hazards)
        {
            Destroy(hazard.gameObject);
        }

    } //end DestroyAllSpawnedObjects
} //end of class  

```

