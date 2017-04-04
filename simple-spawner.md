# **Simple Spawner**

In the code below, we will spawn a prefab gameObject that's added in the inspector, using the public GameObject variable.  
The prefab gameObjects should have an instance of the PickUp Script added as a script component.

The spawned objects must have a Tag that corresponds to the PlayerController Tag in OnTriggerEnter2D\( \).  It may work best to have all item Tags set to 'PickUp'.

Objects to be Spawned in the MiniGame should have the following configuration:

1. GameObject should be a PreFab
2. A SpriteRenderer Component
3. At least 1 Collider2D component with IsTrigger set to true \(can have additional nested Collider2D components if necessary\)
4. A PickUp script component, with the public attributes like: value, type, damage set in the inspector.
5. GameObject: Tag:  PickUp
6. Can have a Rigidbody2D if the spawned object will be moving
7. Can have an Animator component if the object will be animated

```
using UnityEngine;
using System.Collections;
using System.Collections.Generic;

public class Spawner : MonoBehaviour
{

    // The prefab we will spawn
    public GameObject prefab;


    // Use this for initialization
    private int prefabsCount;
    private int pauseTime;

    void Start ()
    {
        prefabsCount = 6;
        pauseTime = 5;
        StartSpawning ();
    }

    public void StartSpawning ()
    {
        for (int i = 0; i < prefabsCount; i++) {
            Invoke ("SpawnPrefab", Random.Range (pauseTime, pauseTime * prefabsCount)); 
        }
    }

    public void SpawnPrefab ()
    {
        Vector3 position = transform.localPosition;
        position.x = Random.Range (-7.6f, 7.6f);
        position.y = Random.Range (-2.4f, -3.7f);
        Instantiate(prefab, position, transform.rotation);
        Debug.Log("Prefab item has been Spawned");
    }

}
```

### Spawn From a List of Prefabs



