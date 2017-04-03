# **Simple Spawner**

In the code below, we will spawn a prefab gameObject that's added in the inspector, using the public GameObject variable.



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

	}

}

```



