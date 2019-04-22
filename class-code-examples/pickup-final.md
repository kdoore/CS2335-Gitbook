###Class Pickup
**updated Apr 10,2019**

```java
using System.Collections;
using System.Collections.Generic;
using UnityEngine;

//combines frontEnd UI and BackEnd 
public class PickUp : MonoBehaviour {

    public ItemInstance itemInstance;

    private int value;

    //read-only property
    public int Value
    {
        get { return value; }
    }

    private void Start()
    {
        this.value = itemInstance.item.value;
    }

    /// <summary>
    /// Adds the item to GameData Inventory
    /// Can be executed by button.onClick
    /// when added as a listener
    /// </summary>
    public void AddItem( ) //can be called onClick for a button
    {
        GameData.instanceRef.AddItem(this.itemInstance);
    }
}//end class PickUp


```


#Hazard Class
This simple class will be used to replace PickUp for Hazard-type objects.


```java
using UnityEngine;


public class Hazard : MonoBehaviour {

    public int value;
	
}
```

#ScorePickUp
This simple class can be used to replace PickUp.cs script component for objects with Collectible Tag, if the item only impacts the score, with no inventory items added.

```java

using UnityEngine;

/// <summary>
/// Score pickup.
/// Add to Prefab that will increase score when collision-trigger with Player
/// </summary>
public class ScorePickUp : MonoBehaviour {

    //set in inspector
    public int Value;
}
```



