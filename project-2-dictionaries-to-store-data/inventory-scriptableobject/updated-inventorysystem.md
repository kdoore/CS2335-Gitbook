#Updated InventorySystem

Updated: 4/30/2019

If you update all scripts below, then your Inventory display all items correctly: 

The initial version of InventorySystem had the InventoryDisplay use ItemInstance as the key for the dictionary of item Counts.  Instead, we want to use item as the dictionary key.  

In the entire project, there is one item, and many itemInstances.  The ItemInstance is a wrapper class that will allow easy expansion to include additional attributes to our inventory items.  So, in that case, if we had good, better, best qualities, then we'd need to decide the best way to display those.

**Required Code Changes - Code listed below**
   - **Inventory.cs**
   -  **InventoryDisplay.cs**
   - **Slot.cs**


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
    public UnityEvent onInventoryUpdate = new UnityEvent();

    /* Inventory START */
    public List<ItemInstance> inventory;  // - initialized in inspector

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
    public bool RemoveItem(Item item)
    {
        bool removed = false;  //loop through until find an instance with item, remove one
        for( int i=0; i< inventory.Count; i++)
        {
            ItemInstance instance = inventory[i];

            if (instance.item == item)
            {
                removed = true;
                inventory.Remove(instance); ///use list remove
            }

        }


        return  removed;  //returns true/false
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

# InventoryDisplay Class


```java

using System.Collections;
using System.Collections.Generic;
using UnityEngine;
using UnityEngine.UI;

//modified from
////https://github.com/Toqozz/blog/blob/master/inventory

public class InventoryDisplay : MonoBehaviour
{
    private List<Slot> inventorySlots;
    private Inventory inventory;
    public Dictionary<Item, int> itemCounts =  new Dictionary<Item, int>(); //dictionary
    public const int NumSlots = 4;
    public int displayedRowPointer=0; //
    Animator visibilityAnimator;
    public bool isVisible = false;

        // Use this for initialization
    void Start()
    {
        inventory = GameData.instanceRef.inventory;
        //inventoryDisplay wants to be notified when inventory changes
        inventory.onInventoryUpdate.AddListener(UpdateDisplay);

        // Load example
        inventorySlots = new List<Slot>();
        //add child slots to the inventorySlots list.
        inventorySlots.AddRange(Object.FindObjectsOfType<Slot>());

        // Maintain some order (just in case it gets screwed up).
        inventorySlots.Sort((a, b) => a.index - b.index);
        SetSlotIndexes();
        PopulateInitial();
        visibilityAnimator = GetComponent<Animator>();
        visibilityAnimator.SetBool("IsVisible", false);
    }

    private void Update()
    {
        if( Input.GetKeyUp( KeyCode.Tab))
        {
            isVisible = !isVisible;
            visibilityAnimator.SetBool("IsVisible", isVisible);
        }
    }

    //update itemCount dictionary: items, counts - get data from
    //inventory
    //condense data into key-value pairs:  item: count
    private void UpdateItemCounts()
    {
        itemCounts.Clear(); //clear out dictionary
         //for all items-slots in the inventory
         //count the number of identical items - 
         //put unique item in dictionary/ with count as value
        for( int i=0; i< inventory.inventory.Count; i++)
        {
            //for each item-slot in data inventory
            //if the slot has an item
            //add key to dictionary, update count

            if (!inventory.SlotEmpty(i)) //is there an item in the inventory for each list item?
            {
                Item item = inventory.inventory[i].item;
                int count;
                if (itemCounts.TryGetValue(item, out count))
                {
                    itemCounts[item] = ++count; //if already added, increment count
                }
                else
                {
                    itemCounts.Add(item, 1); //add item
                }
            }
        }
    }

    private void PopulateInitial()
    {
        UpdateItemCounts(); //populate dictionary
        if( itemCounts.Count > 0) { //something in the dictionary
        int index = 0;
            //get the keys (all existing items) 
            Dictionary<Item, int>.KeyCollection keys = itemCounts.Keys;
            //loop through all instance-count pairs
            foreach (Item instance in keys)
            {
                if (index < NumSlots) //fill the first 4 slots
                {
                    int value = itemCounts[instance];//use current key-item to get the count (value)
                   // Debug.Log("PopulateInitial:item count" + value );
                    if (instance != null)
                    {
                        //Update the slot, by adding an item
                        inventorySlots[index].SetItem(instance, value);
                        index++; //move to next slot
                    }
                }
                else
                {

                  //  Debug.Log("more items than slots, some not displayed");
                }
            }
        }

    } //end method

    void SetSlotIndexes()
    {
        foreach( Slot slot in inventorySlots)
        {
            slot.index = inventorySlots.IndexOf(slot);
        }
    }

    public void RemoveItem(Item item, int index )
    {
        int count;
        if(itemCounts.TryGetValue( item, out count))
        {
            if( count <= 1) //if the last one
            {

                itemCounts.Remove(item); //remove from dictionary itemCounts
                inventorySlots[index].RemoveItem(index);//remove from slot display
                inventory.RemoveItem(item); //remove from data inventory
            }
            else
            {
                itemCounts[item] = count - 1;  //decrease
                inventorySlots[index].SetItem(item, count-1);
                inventory.RemoveItem(item);
            }
          
        }
    }


    public void UpdateDisplay()
    {
        Clear();
        PopulateInitial();
    }

    //Adds an item to the itemCount dictionary
    //or updates the count if item was already in the dictionary
    void AddToCounts(Item item)
    {
        int count;
        if (itemCounts.TryGetValue(item, out count))
        {
            itemCounts[item] = ++count;
            Debug.Log("itemCount " + item.itemName + " " + count);
        }
        else if (itemCounts.Count <= NumSlots) //we still have an available slot
        {
            itemCounts.Add(item, 1); //add first item
        }
    }

   private void Clear()
    {
        for (int i = 0; i < inventorySlots.Count; i++)
        {
            inventorySlots[i].RemoveItem(i);
        }
    }

    //unregister listener when this object is destroyed 
    private void OnDisable()
    {
        inventory.onInventoryUpdate.RemoveListener(UpdateDisplay);
    }

} //end class InventoryDisplay

```


#Slot Class



```java
using UnityEngine;
using UnityEngine.UI;
using UnityEngine.EventSystems;

//modified from
////https://github.com/Toqozz/blog/blob/master/inventory

public class Slot : MonoBehaviour, IPointerClickHandler
{
    InventoryDisplay inventoryDisplay; 
    public int index = 0; //is auto-set within PhysicalInventory
    public Item item = null;    // Inventory backend representation.
   // public GameObject prefabInstance = null;    // Inventory frontend representation.
    public Image childImage;
    public Text childText;
 
    void Start()
    {
        inventoryDisplay = FindObjectOfType<InventoryDisplay>();

    }


    //Detect if a click occurs
    public void OnPointerClick(PointerEventData pointerEventData)
    { //check to see if an item is in the slot!
        if (item != null)
        {
            item.Use();
            inventoryDisplay.RemoveItem(item, index);
            //TODO USE and REMOVE ITEM
            //Output to console the clicked GameObject's name and the following message. You can replace this with your own actions for when clicking the GameObject.
            Debug.Log(name + " Game Object Clicked!");
        }
    }

    // TODO: it would be better if we used SetActive() etc rather than Instantiate/Destroy.
    // Use this method to set a slot's item.
    // The slot will automatically instantiate the gameobject associated with the item.
    public void SetItem(Item  item, int count)
    {
        if (item != null)
        {
            this.item = item;
            this.childImage.sprite = item.sprite;
            this.childImage.preserveAspect = true;
            this.childImage.color = Color.white;
            this.childText.color = Color.white;
            this.childText.text = count.ToString();
        }
    }

    // Remove the item from the slot, and destroy the associated gameobject.
    public void RemoveItem(int index)
    {
        if (this.index == index) //check it is the slot's current index
        {
            this.item = null;
            this.childImage.sprite = null;
            this.childImage.color = Color.clear;
            this.childText.color = Color.clear;
        }
    }
} //end class Slot


```

