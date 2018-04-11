#GameData with UnityEvent

This version of the GameData script uses a custom UnityEvent to notify listeners that some change happened to the player's data.

This version also contains:
    - Inventory: Dictionary< PickupType, int> inventory
    - levelScore variable, LevelScore Property
    - health variable, Health Property


**UnityEvent**
This example uses a simple UnityEvent: `onPlayerDataUpdate,` to notify any Listeners that the playerData has been updated.  Potential Listener objects include the LevelManager script, the PlayerStatsDisplay, and the InventoryDisplay.  Make sure to add:  using `UnityEngine.Events;`, to the top of your script

**GameObject:**  GameData should be added to the GameManager, empty gameObject, in the BeginScene, and in the MiniGame scene for easy testing of the game.

```java
using System.Collections;
using System.Collections.Generic;
using UnityEngine;
using UnityEngine.Events;

//Class to create a singleton object
//this will be attached to the GameManager in BeginScene and
//persisted throughout the game
//This object will maintain the important data about the gameplay experience
public class GameData : MonoBehaviour
{
    public static GameData instanceRef;  //singleton reference variable

    public UnityEvent onPlayerDataUpdate;   //custom Unity Event

    public Dictionary<PickupType, int> inventory = new Dictionary<PickupType, int>();

    private int health;
    private int totalScore;
    private int levelScore;

    /// <summary>
    /// property
    /// </summary>
    /// <value>The total score.</value>
    public int TotalScore
    {  //read only
        get { return totalScore; }
    }

    /// <summary>
    ///  read-only property
    /// </summary>
    /// <value>The health.</value>
    public int Health
    {
        get { return health; }
    }

    /// <summary>
    /// Gets or sets the level score.
    /// </summary>
    /// <value>The level score.</value>
    public int LevelScore{
        get { return levelScore; }
        set { levelScore = value; }
    }

    void Awake()
    {  
       
        //create singleton
        if (instanceRef == null)
        {
            instanceRef = this;
            DontDestroyOnLoad(gameObject);  //the gameObject this is attached to 
        }
        else
        {   //
            DestroyImmediate(gameObject);
            Debug.Log("Destroy Imposter GameObject");
        }

        //initilize these instance variables
        health = 100;
        totalScore = 0;
        levelScore = 0;

        /////////Initialize Custom EVENT
        if (onPlayerDataUpdate == null)//test to see if it has been initialized
        {
            onPlayerDataUpdate = new UnityEvent();
        }

    }
    // Use this for initialization
    void Start()
    {
        //health = 100;  MOVE THIS TO Awake( )

        //totalScore = 0;  MOVE THIS TO Awake( )
    }

    ////Called in Player controller when the player collides with a pickup    
    public void Add(PickUp item)
    {
        totalScore += item.value;  // update totalScore by the value of this current item
        levelScore += item.value;
        Debug.Log("Adding item value to totalScore, totalScore =  " + totalScore);
       
       
   ///THE INVENTORY DICTIONARY 
   ///Add item to dictionary
        int count = 0;
        if( inventory.TryGetValue(item.type, out count)){
            count++;
            inventory[item.type] = count; ///update the dictionary's value
        }else{
            inventory.Add(item.type, 1);  //add a new entry to the dictionary
        }
        
        ///THE EVENT
        if(onPlayerDataUpdate != null){ /////Are there any objects registered as listeners
onPlayerDataUpdate.Invoke(); ///Invoke the event
}

    }
    
    // end Add()

    public void TakeDamage(int damage)
    {
        health -= damage;
        if (health <= 0)
        {
            Debug.Log("GameOver due to low health");
        }

        if (onPlayerDataUpdate != null)
        {   /////Are there any objects registered as listeners
            onPlayerDataUpdate.Invoke();  ///Invoke the event
        }
    }

    //called when health == 0 and scene is reloaded
    public void ResetGameData(){
        health = 100;
        levelScore = 0;
        totalScore = 0;
    }
}
```

