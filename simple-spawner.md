# Simple Spawner

In the code below, we will spawn a prefab gameObject that's added in the inspector, using the public GameObject variable.  
The prefab gameObjects should have an instance of the PickUp Script added as a script component.

###Objects to Spawn - PickUp Prefabs
PickUp Prefabs to be Spawned in the MiniGame should have the following configuration:  [See PickUp PreFabs](/pickup_items.md)

	- GameObject should be a PreFab
	- A SpriteRenderer Component
	- At least 1 Collider2D component with IsTrigger set to true \(can have additional nested Collider2D components if necessary\)
	- A PickUp script component, with the public attributes like: value, type set in the inspector.
	- GameObject: Tag:  "Collectible" or "Hazard"
	- Can have a Rigidbody2D if the spawned object will be moving
	- Can have an Animator component if the object will be animated
	
	**Important:** The PickUp Prefabs must have a Tag set that corresponds to the PlayerController Tag in OnTriggerEnter2D\( \).  



###Simple Spawner
The code below creates a simple spawner

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
public int numToSpawn=10;
public float chanceToSpawnBad = .10f; //10% chance
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
	Invoke("SpawnPrefab", Random.Range(pauseTime, pauseTime * 2.0f * i)); //more delay for each value of i in the for-loop
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

} // end Spawner
```


### Called from LevelManager

The Level Manager can start the Spawning.

# Spawn From a List of Prefabs

The code below introduces a version of the Spawner that can spawn from a list of prefabs.  



```java
using UnityEngine;
using System.Collections;
using System.Collections.Generic;

public class Spawner : MonoBehaviour
{

	// The prefab we will spawn
	[Header("Set in Inspector")]
	public List<GameObject> prefabs = new List<GameObject> ();

	// Use this for initialization
	
	private int pauseTime;
	private int currentIndex;  //which one to spawn
 
 	//property with write permissions
 	//can be set from LevelManager
 	public int CurrentIndex{
 		set{ currentIndex = value; }
 	}
 	

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
	///  Called from Level Manager // pass in index of what we want spawned, how many we want to spawn
	/// </summary>
	/// <param name="prefabIndex">Prefab index.</param>
	/// if Level is 1, spawn prefab 0
	public void StartSpawning (int levelIndex, int numToSpawn)
	{ 
		currentIndex = levelIndex - 1;
		if (prefabs.Count > 0) {  //make sure there are some gameObjects to spawn
			for (int i = 0; i < numToSpawn; i++) {

				Invoke ("SpawnSpecificPrefab", Random.Range (pauseTime, pauseTime * i)); //delay 
			}
		}
	}

	//spawns 1 prefab
	public void SpawnRandomPrefab ()
	{
		currentIndex = Random.Range (0, prefabs.Count);//gives values, 0, 1,...up to prefabs.Count-1
		
		Vector3 position = transform.localPosition;
		position.x = Random.Range (-7.6f, 7.6f);
		position.y = Random.Range (-2.4f, -3.7f);
		Instantiate (prefabs [currentIndex], position, transform.rotation);
		Debug.Log ("Something is spawned index: " + currentIndex);
	}

	/// <summary>
	/// Spawns the specific prefab. - Specific prefab corresponds to a 
	/// specific level number.
	/// currentIndex is a public class instance variable that 
	///can be set from another class like LevelManager
	/// </summary>
	public void SpawnSpecificPrefab ()
	{
		Vector3 position = transform.localPosition;
		position.x = Random.Range (-7.6f, 7.6f);
		position.y = Random.Range (-2.4f, -3.7f);
		Instantiate (prefabs [currentIndex], position, transform.rotation);
		Debug.Log ("Something is spawned index: " + currentIndex);
	}
	
	//overloaded version takes an integer index value
	public void SpawnSpecificPrefab ( int index)
	{
		Vector3 position = transform.localPosition;
		position.x = Random.Range (-7.6f, 7.6f);
		position.y = Random.Range (-2.4f, -3.7f);
		Instantiate (prefabs [index], position, transform.rotation);
		Debug.Log ("Something is spawned index: " + currentIndex);
	}



}

```

##Using Random Numbers in Unity

[Unity Manual: Using Random Numbers](https://docs.unity3d.com/Manual/RandomNumbers.html) 

>Choosing a Random Item from an Array
Picking an array element at random boils down to choosing a random integer between zero and the array’s maximum index value (which is equal to the length of the array minus one). This is easily done using the built-in Random.Range function:-



```
 var element = myArray[Random.Range(0, myArray.Length)];
 
 var element = myList[ Random.Range( 0, myList.Count)];
```



>Note that Random.Range returns a value from a range that includes the first parameter but excludes the second, so using myArray.Length, or myList.Count here gives the correct result.

