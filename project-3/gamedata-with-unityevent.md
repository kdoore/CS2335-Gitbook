#GameData with Inventory and Custom UnityEvent

This version of the GameData script uses a custom UnityEvent to notify listeners that some change happened to the player's data.

This version also contains:
    - Inventory: Dictionary< PickupType, int> inventory
    - levelScore variable, LevelScore Property
    - health variable, Health Property
    - lives variable, Lives Property


**UnityEvent - not included in code below**
This example uses a simple UnityEvent: `onPlayerDataUpdate,` to notify any Listeners that the playerData has been updated.  Potential Listener objects include the LevelManager script, the PlayerStatsDisplay, and the InventoryDisplay.  Make sure to add:  using `UnityEngine.Events;`, to the top of your script

**GameObject:**  GameData should be added to the GameManager, empty gameObject, in the BeginScene, and in the MiniGame scene for easy testing of the game.

```java  
//updated 3/25/19 
using System.Collections;
using System.Collections.Generic;
using UnityEngine;

//Singleton - one and only 1 ever in existance
//global variable to allow easy access
public class GameData : MonoBehaviour {


    /// static means it belongs to the Class, and not to an object instance of the class

    public static GameData instanceRef;  ///Global variable 
    private int score;
    private int health;

    public Dictionary<PickupType, int> inventory = new Dictionary<PickupType, int>();

    //Properties - Support Encapsulation - protect inner workings of our class
    public int Score{
        get{ return score;    }   //read only access
      }

    public int Health
    {
        get { return health; }   //read only access
    }

    private void Awake()
    {
        health = 100; //initialize
        score = 0;

        if(instanceRef == null){
            instanceRef = this; //point to itself
            DontDestroyOnLoad(this.gameObject);  //this will never be destroyed
        }else{
            Destroy(this.gameObject);
            Debug.Log("Duplicate GameData is Destroyed");
        }
    }

    /// <summary>
    /// Add the specified item.
    /// Overloaded method
    /// takes the full PickUp item as input parameter
    /// </summary>
    /// <param name="item">Item.</param>
    public void AddItem( PickUp item)
    {
        Debug.Log("Item added " + item.type);
        ///THE INVENTORY DICTIONARY
        ///Add item to dictionary - based on it's PickupType value.
        ///The PickupType is used to determine which item to display in the InventoryDisplay script.
        int count = 0; //variable to hold out parameter value, if the Dictionary already has the item.type key.
        if (inventory.TryGetValue(item.type, out count))
        {
            count++; //item already in dictionary, add one to count
            inventory[item.type] = count; ///update the dictionary's value
        }
        else
        {
            inventory.Add(item.type, 1); //add a new entry to the dictionary
        }

    }

    /// <summary>
    /// Add the specified value.
    /// Overloaded method
    /// takes the PickUp item's value as input parameter
    /// </summary>
    /// <param name="value">Value.</param>
    public void Add(int value)
    {
        score += value;
        Debug.Log("Score is updated " + score);

    }

    public void TakeDamage( int value){
        health -= value;
        Debug.Log("Health is updated " + health);
    }

    public void ResetGameData()
    {
        health = 100; //initialize
        score = 0;
    }

} //end class

```

