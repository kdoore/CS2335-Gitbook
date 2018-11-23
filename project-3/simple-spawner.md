#Simple Spawner
This code creates the simplest possible spawner
.
This code will be attached to an empty gameObject: Spawner.  The StartSpawning method can be executed from the LevelManager, rather than from Start...which will cause spawning to begin when the scene is first loaded. 

Make sure to check the Transform.position.Z value of the Spawner gameObject, make sure it is 0, not -10.  The spawned prefabs will be initialized with this value for Transform.position.Z. Then the objects will be behind the main camera and won't be visible when the scene plays.  

You'll customize this to meet your needs.  

```java

using UnityEngine;
using System.Collections;
using System.Collections.Generic;

public class Spawner : MonoBehaviour
{

    // The prefab we will spawn
   [Header("Set in Inspector")]
    public GameObject prefabGood, prefabBad;

    // Use this for initialization

    private int pauseTime=2;//wait 2 sec before starting
    public int numToSpawn=3;
    public float chanceToSpawnBad = .01f;
    public float xRange = 8.0f;
    public float yRangeTop = -2.0f;
    public float yRangeBottom = -3.5f;
    public bool activeSpawning = false;

    void Start()
    {
       activeSpawning = true;
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
    if( activeSpawning){
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
    } //end SpawnPrefab method
    
    
    ///This method can be called from any other script using the Spawner object, to destroy all spawned objects with Tags as shown.

    public void DestroyAllSpawnedObjects()
    {
    GameObject[] goodItems =GameObject.FindGameObjectsWithTag("Collectible");
    Debug.Log("Destroy goodObjects spawner" + goodItems.Length);
        foreach (var item in goodItems)
        {
            Destroy(item);
        }
    GameObject[] badItems =GameObject.FindGameObjectsWithTag("Hazard");
    Debug.Log("Destroy badObjects spawner" + badItems.Length);
        foreach (var item in badItems)
        {
        Destroy(item);
        }
    } // end DestroyAll

    
} // end Spawner
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


