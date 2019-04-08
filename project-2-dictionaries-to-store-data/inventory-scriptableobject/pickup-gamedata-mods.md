#PickUp, GameData Mods


###Class Pickup



```java
using System.Collections;
using System.Collections.Generic;
using UnityEngine;


//combines frontEnd UI and BackEnd 
public class PickUp : MonoBehaviour {

    public ItemInstance itemInstance;

    /// <summary>
    /// Adds the item to GameData Inventory
    /// Can be executed by button.onClick
    /// when added as a listener
    /// </summary>
    public void AddItem( ) //can be called onClick for a button
    {
        GameData.instanceRef.Add(this.itemInstance);
    }
}
```


## GameData Changes:  with Inventory, Add(ItemInstance)
See changes in code below:

```java
//New Variable: 
public Inventory inventory;
//New Method:  
public void Add( ItemInstance){     }
```

##Class GameData with Inventory, Add(ItemInstance)

```java
using System.Collections;
using System.Collections.Generic;
using UnityEngine;
using UnityEngine.Events;

//Singleton - one and only 1 ever in existance
//global variable to allow easy access
public class GameData : MonoBehaviour {

    /// static means it belongs to the Class, and not to an object instance of the class

    public static GameData instanceRef;  ///Global variable 
    private int score;
    private int health;
    public Inventory inventory;

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
    public void Add( ItemInstance itemInstance)
    {
        //score += itemInstance.value;
        inventory.InsertItem(itemInstance);
       
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

}

```


