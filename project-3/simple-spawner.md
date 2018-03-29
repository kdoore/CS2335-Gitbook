#Simple Spawner
This code creates the simplest possible spawner.


```java

using UnityEngine;
using System.Collections;
using System.Collections.Generic;

public class Spawner : MonoBehaviour
{

    // The prefab we will spawn
    public GameObject prefabGood, prefabBad;

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
    if (rand < chanceToSpawnBad)
        {
        Instantiate(prefabBad, position, transform.rotation);
        }else{
        Instantiate(prefabGood, position, transform.rotation);
        }
    }
}
```

