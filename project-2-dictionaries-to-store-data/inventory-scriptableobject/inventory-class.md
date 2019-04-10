#Inventory Class




```java
using System.Collections;
using System.Collections.Generic;
using UnityEngine;
using UnityEngine.Events;

//modified from
////https://github.com/Toqozz/blog/blob/master/inventory

[System.Serializable]
public class Inventory : ScriptableObject
{
    //custom event - invoked when item added to Inventory
    public UnityEvent onInventoryUpdate = new UnityEvent();

    public List<ItemInstance> inventory;  // - initialized in inspector

    //Unity Event, initializes or clears the List<>
    public void OnEnable()
    {
        if (inventory == null)
        {
            inventory = new List<ItemInstance>();
        }
        else
        {
            inventory.Clear();
        }
    }

    public bool SlotEmpty(int index)
    {
        if (inventory[index] == null || inventory[index].item == null)
        {
            Debug.Log(" empty slot");
            return true;
        }
        return false;
    }

    // Remove an item at an index if one exists at that index.
    public bool RemoveItem(ItemInstance item)
    {
        return (inventory.Remove(item));  //returns true/false
    }

    // Insert an item, return the index where it was inserted.  -1 if error.
    public void InsertItem(ItemInstance item)
    {
        Debug.Log("item added to inventory " + item.item.name);
        inventory.Add(item); //add to list 

        ///Broadcast event to notify listeners
        if (onInventoryUpdate != null)
        {
            onInventoryUpdate.Invoke();
        }

    } //end method

} //end class
```

