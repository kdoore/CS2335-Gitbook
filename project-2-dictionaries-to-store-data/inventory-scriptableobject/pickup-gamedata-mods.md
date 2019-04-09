#PickUp, GameData, PlayerController Mods

The following scripts contain updated code required for integrating the new GameData Inventory inventory, from the new Inventory System.  

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
    public void AddItem( ) //can be called onClick for a button to add an item to inventory
    {
        //updated preferred method 
        GameData.instanceRef.AddItem(this.itemInstance);
        
        //removed 
        //GameData.instanceRef.Add(this.itemInstance);

    }
}
```


## GameData Changes:  with Inventory, Add(ItemInstance)
See changes in code below:

```java
//New Variable: 
public Inventory inventory;
//New Method:  
public void AddItem( ItemInstance){     }

```

##Updated Class GameData with Inventory, Add(ItemInstance)

```java
using System.Collections.Generic;
using UnityEngine;
using UnityEngine.Events;

//Singleton - one and only 1 ever in existance
//global variable to allow easy access
public class GameData : MonoBehaviour {


    /// static means it belongs to the Class, and not to an object instance of the class
    public Inventory inventory; ///Set in the inspector in BeginScene

    public static GameData instanceRef;  ///Global variable 

    public UnityEvent onPlayerDataUpdate = new UnityEvent();//calling constructor for our UnityEvent object

    private int score;
    private int health;

   
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

    public void Add( int value){
        score += value;
        Debug.Log("Score is updated " + score);
        //Score has changed
        if(onPlayerDataUpdate != null)// there are no listeners, the list is empty
        {
            onPlayerDataUpdate.Invoke(); //Broadcast the event ( execute the listener methods)
         //Invoke here means execute all methods on the listner list.
        }
    }

    //Adds an item to the inventory - list
    //remove this and use the method below
    public void Add(ItemInstance item)    
    {
    inventory.InsertItem(item);
    Debug.Log("Add an item to the inventory " +         item.item.itemName);
    }

    //Preferred - Adds an item to the inventory - list
    public void AddItem(ItemInstance item)
    {
        inventory.InsertItem(item);
        Debug.Log("Add an item to the inventory " + item.item.itemName);
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

#Updated code for PlayerController 
