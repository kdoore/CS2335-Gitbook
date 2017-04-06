# Spawn Prefab GameObjects

In order to dynamically spawn game objects in a scene, we'll need to first create a Prefab of a gameObject.  A prefab is an asset that we can include in any scene, we can consider that it is a preconfigured gameObject, which can include any valid components such as script and animator as pre-configured components which are instantiated when an instance of a Prefab is instantiated in a scene.

### Melting Crystal - Specialized PickUp

In our mini-game, we want to spawn animated crystals - which melt.  So we need to create a 2D sprite gameObject, then we'll need to attach an animator component and create an animation clip that reduces the sprite's transform scale.x, scale.y values using keyframes.  We want the crystal to generate a custom event when it's destroyed, so we're using PickUp as a base-class and we've defined an OnDiedHandler delegate and an OnDied event, we will put this logic in a custom script: crystalController.cs

```
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

```
using UnityEngine;
using System.Collections;

public class CrystalSpawner : MonoBehaviour {

  // The prefab that we're going to spawn 
    public PickUp prefab;  //use baseclass type

    private int prefabsCount;
    private int pauseTime;

    // Delegates to notify the UI
    public delegate void OnSpawn();
    public event OnSpawn onSpawn;

    // Explicitly Create the first crystals in unity start
    void Start () {
        prefabsCount = 2;
        pauseTime = 1;
        for (int i = 0; i < prefabsCount; i++){
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

        // Notify if any UI is listening that we just had a spawn event
        if (onSpawn != null)    //spawn event notification
            onSpawn();
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

### MiniGame.cs - The UI View

Since we've been using our State.cs classes for implementing the logic for the game UI, for now we can say that the MiniGame.cs class, which Implements IStateBase and which represents the current activeState for our State-Machine Framework.  Later we can create a Prefab to represent our Player heads-up display for presenting player game stat information.

Inside the UI-View, we want to receive notification of spawn events and we want to receive notification of data events so we can display these on the screen.

