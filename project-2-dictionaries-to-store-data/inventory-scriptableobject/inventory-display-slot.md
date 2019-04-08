#Inventory Display and Slot Classes


##Hierarchy and Inspector Panel 
###Images for Slot and Inventory Display

![](/assets/Screen Shot 2019-04-08 at 3.14.31 PM.png)
Slot Inspector - Slot Script
![](/assets/Screen Shot 2019-04-08 at 3.28.00 PM.png)

##Add GridLayout Component, Configure as below
![](/assets/Screen Shot 2019-04-08 at 3.23.49 PM.png)

Test Buttons - Button with PickUp-Item as a child
![](/assets/Screen Shot 2019-04-08 at 3.23.05 PM.png)


Configure Button OnClick to execute PickUp method:  AddItem( )
![](/assets/Screen Shot 2019-04-08 at 3.26.05 PM.png)

##Class Slot

```java
using System.Collections;
using System.Collections.Generic;
using UnityEngine;
using UnityEngine.UI;
using UnityEngine.EventSystems;


public class Slot : MonoBehaviour, IPointerClickHandler
{
    InventoryDisplay inventoryDisplay; 
    public int index = 0; //is auto-set within PhysicalInventory
    public ItemInstance itemInstance = null;    // Inventory backend representation.
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
        if (itemInstance != null)
        {
            itemInstance.item.Use();
            inventoryDisplay.RemoveItem(itemInstance, index);
            //TODO USE and REMOVE ITEM
            //Output to console the clicked GameObject's name and the following message. You can replace this with your own actions for when clicking the GameObject.
            Debug.Log(name + " Game Object Clicked!");
        }
    }

    // TODO: it would be better if we used SetActive() etc rather than Instantiate/Destroy.
    // Use this method to set a slot's item.
    // The slot will automatically instantiate the gameobject associated with the item.
    public void SetItem(ItemInstance instance, int count)
    {
        if (instance != null)
        {
            this.itemInstance = instance;this.childImage.sprite = instance.item.sprite;
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
            this.itemInstance = null;
            this.childImage.sprite = null;
            this.childImage.color = Color.clear;
            this.childText.color = Color.clear;
        }
    }
}

```
##Class Inventory Display




```java
using System.Collections;
using System.Collections.Generic;
using UnityEngine;

public class InventoryDisplay : MonoBehaviour
{
    List<Slot> inventorySlots;
    Inventory inventory;
    public Dictionary<ItemInstance, int> itemCounts =  new Dictionary<ItemInstance, int>(); //dictionary
    public const int NumSlots = 4;

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
                    Debug.Log("PopulateInitial:item count" + value );
                    if (instance != null)
                    {
                        //Update the slot, by adding an item
                        inventorySlots[index].SetItem(instance, value);
                        index++; //move to next slot
                    }
                }
                else
                {
                    Debug.Log("more items than slots, some not displayed");
                }
            }
        }
        else { Debug.Log("No items to put in inventory slots" + itemCounts.Count); }
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
}

```


