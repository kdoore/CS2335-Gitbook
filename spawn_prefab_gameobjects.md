# Event Driven Spawner

In order to dynamically spawn game objects in a scene, we'll need to first create a Prefab of a gameObject.  A prefab is an asset that we can include in any scene, we can consider that it is a preconfigured gameObject, which can include any valid components such as script and animator as pre-configured components which are instantiated when an instance of a Prefab is instantiated in a scene.

### Melting Crystal - Specialized PickUp

In our mini-game, we want to spawn animated crystals - which melt.  So we need to create a 2D sprite gameObject, then we'll need to attach an animator component and create an animation clip that reduces the sprite's transform scale.x, scale.y values using keyframes.  We want the crystal to generate a custom event when it's destroyed, so we're using PickUp as a base-class and we've defined an OnDiedHandler delegate and an OnDied event, we will put this logic in a custom script: crystalController.cs

```java
using UnityEngine;
using System.Collections;

public class CrystalController : PickUp {

    // Life variables
    private float minLifeTime;
    private float maxLifeTime;

//Declare custom delegate and event - so crystal can notify subscribers ( spawner ) when 
// the crystal has died.

    new public delegate void onDiedHandler( PickUp thisPickup);
    new public event onDiedHandler onDied; 

    void Start () {
        // Self-destruct after random time by calling base class died method
        minLifeTime = 100.0f;
        maxLifeTime = 400.0f;
        type = PickupType.crystal;
         Invoke ("Died", Random.Range(minLifeTime, maxLifeTime));  //
    }
}
```

### CrystalSpawner - Manage the Spawning

We need to create a custom scritp that can spawn our game objects:  
This script needs to be attached to an empty gameObject in our scene.  
In addition, the CrystalSpawn class has also defined an EventHandler: OnSpawn\(\) and an associated Event for notification: OnSpawn  
This will allow any gameObject in the scene to register for, and receive notifications every time that we spawn a new prefab.

```java
using UnityEngine;
using System.Collections;

public class CrystalSpawner : MonoBehaviour {

  // The prefab that we're going to spawn 
    public PickUp prefab;  //use baseclass type
    
    private int pauseTime;

  

    // Explicitly Create the first crystals in unity start
    void Start () {
        pauseTime = 1;
        for (int i = 0; i < prefabs.Count; i++){
            Invoke("SpawnPrefab", Random.Range(pauseTime+1, pauseTime+5)); 
        }
    }

    // Helper function to spawn prefabs and to register as a subscriber
    // to the crystal OnDied event notification
    public void SpawnPrefab (){ //this is the actual SpawnEvent

        //select position to spawn based on spawner position
        Vector3 pos = transform.position;
        pos.x = Random.Range(-6.0f, 6.0f);  //figure these range values based on scene geometry - move temp prefab to min, max positions
        pos.y = Random.Range (-4.8f, -4.2f);

        // Spawn the Pickup and assign to a temp object reference
        PickUp newPickup = (PickUp)Instantiate(prefab,pos,transform.rotation);

        // Subscribe so the other class handles notification automatically
        ///the spawn object has it's OnPickUpDied function added to the list of subscribers

        newPickup.onDied += OnPickUpDied; //when this crystal dies, please notify this spawn class

    }


    // OnPickUpDied is an EventHandler Function that will be called automatically when the pickup object instance dies
    // This allows it to spawn a new prefab instance, and then 
    // we need to remember to unregister

    public void OnPickUpDied (PickUp thisPickUp){

        // We unsubscribe to avoid memory leaks, we need to use the
        //object reference passed into this method in order to unregister
        //as a listener to the onDied event on this crystal
        thisPickUp.onDied -= OnPickUpDied;

        // Spawn a new crystal 
        Invoke("SpawnPrefab", Random.Range(pauseTime, pauseTime+3));
    }

}
```



