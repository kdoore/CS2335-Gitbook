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

```java
using UnityEngine;
using System.Collections;
using System.Collections.Generic;

public class Spawner : MonoBehaviour
{

    // The prefab we will spawn
    public GameObject prefab;


    // Use this for initialization
   
    private int pauseTime;

    void Start ()
    {
      pauseTime = 5;
       // StartSpawning ();  no longer start spawning in Start, let LevelManager call StartSpawning()
    }

    public void StartSpawning ()
    {
        for (int i = 0; i < prefabs.Count; i++) {
            Invoke ("SpawnPrefab", Random.Range (pauseTime, pauseTime * 2.0f)); 
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

### Called from LevelManager

The Level Manager can start the Spawning.

### Spawn From a List of Prefabs

The code below introduces a version of the Spawner that can spawn from a list of prefabs.  

```java
using UnityEngine;
using System.Collections;
using System.Collections.Generic;

public class Spawner : MonoBehaviour
{

	// The prefab we will spawn
	public List<GameObject> prefabs = new List<GameObject> ();

	// Use this for initialization
	
	private int pauseTime;
	private int currentIndex;

	void Start ()
	{
		pauseTime = 5;
		//StartSpawning (); //call in LevelManager
	}

	public void StartSpawning ()
	{ 

		if (prefabs.Count > 0) {  //make sure there are some gameObjects to spawn
			for (int i = 0; i < prefabs.Count; i++) {
				Invoke ("SpawnRandomPrefab", Random.Range (pauseTime, pauseTime * 2.0f)); 
			}
		}
	}

	/// <summary>
	///  Called from Level Manager // pass in index of what we want spawned
	/// </summary>
	/// <param name="prefabIndex">Prefab index.</param>
	/// if Level is 1, spawn prefab 0
	public void StartSpawning (int levelIndex)
	{ 
		currentIndex = levelIndex - 1;
		if (prefabs.Count > 0) {  //make sure there are some gameObjects to spawn
			for (int i = 0; i < prefabsCount; i++) {

				Invoke ("SpawnSpecificPrefab", Random.Range (pauseTime, pauseTime * prefabsCount)); 
			}
		}
	}

	public void SpawnRandomPrefab ()
	{
		currentIndex = Random.Range (0, prefabs.Count);//values, 0, 1,...upto prefabs.Count-1
		Vector3 position = transform.localPosition;
		position.x = Random.Range (-7.6f, 7.6f);
		position.y = Random.Range (-2.4f, -3.7f);
		Instantiate (prefabs [currentIndex], position, transform.rotation);
		Debug.Log ("Something is spawned index: " + currentIndex);
	}

	/// <summary>
	/// Spawns the specific prefab. - Specific prefab corresponds to a 
	/// specific level number.
	/// </summary>
	public void SpawnSpecificPrefab ()
	{
		Vector3 position = transform.localPosition;
		position.x = Random.Range (-7.6f, 7.6f);
		position.y = Random.Range (-2.4f, -3.7f);
		Instantiate (prefabs [currentIndex], position, transform.rotation);
		Debug.Log ("Something is spawned index: " + currentIndex);
	}

}

```



