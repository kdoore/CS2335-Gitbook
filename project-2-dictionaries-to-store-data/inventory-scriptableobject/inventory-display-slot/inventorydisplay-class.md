#InventoryDisplay Class

###UI-Animation logic - Configure or Disable.
The image below shows code logic in the InventoryDisplay.cs script that animates visibility of the InventoryDisplay when the user presses the `Tab` key. You'll need to decide to configure the animation or you must delete the associated code, or you will have errors in scenes where you use the InventoryDisplay.

**To Disable Animation**
If you choose not to implement the UI-Animation, then you'll
want to disable or remove the code associated with the Animator component - othewise you will have errors when the the InventoryDisplay is included in a scene. 

Delete the Animator declaration line of code in order to identify and disable all code associated with Animator functionality

```java 
Animator visibilityAnimator; ////DELETE THIS LINE - To disable animator
```

**Delete or disable code with errors, shown highlighted in image below**

![](/assets/Screen Shot 2019-04-12 at 9.59.47 AM.png)

**updated Apr 10, 2019**

```java

using System.Collections;
using System.Collections.Generic;
using UnityEngine;
using UnityEngine.UI;

//modified from
////https://github.com/Toqozz/blog/blob/master/inventory
//updated Apr 10, 2019


public class InventoryDisplay : MonoBehaviour
{
    private List<Slot> inventorySlots;
    private Inventory inventory;
    public Dictionary<ItemInstance, int> itemCounts =  new Dictionary<ItemInstance, int>(); //dictionary
    public const int NumSlots = 4;
    public int displayedRowPointer=0; // not used, can be removed
    Animator visibilityAnimator;  //disable if not using UI-animation
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
                ItemInstance item = inventory.inventory[i];
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
            Dictionary<ItemInstance, int>.KeyCollection keys = itemCounts.Keys;
            //loop through all instance-count pairs
            foreach (ItemInstance instance in keys)
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

    public void RemoveItem(ItemInstance item, int index )
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
    void AddToCounts(ItemInstance item)
    {
        int count;
        if (itemCounts.TryGetValue(item, out count))
        {
            itemCounts[item] = ++count;
            Debug.Log("itemCount " + item.item.itemName + " " + count);
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

