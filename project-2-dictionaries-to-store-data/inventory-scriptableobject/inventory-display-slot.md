#Prefabs: Inventory Display and Slot 
##Inventory Front-End:

**Unity Package file with Prefab of UI Display GameObjects**
Link: [UI-DisplayPrefab_Package-S19](https://utdallas.box.com/v/UI-InventoryDisplay-S19)

##InventoryDisplay and Slot Prefab GameObjects:

###Inventory Display Prefab
![](/assets/Screen Shot 2019-04-08 at 3.14.31 PM.png)

- **PhysicalInventory** is a  UI-Panel GameObject 
    - **Monobehaviour Components** on the **PhysicalInventory:**  
        - **Grid-Layout**
        - **InventoryDisplay.cs**
        - **Optional:  Animator Component** - For Animated UI
- **4- Slot Prefabs as Children**
    
** Configure GridLayout as shown below:**
![](/assets/Screen Shot 2019-04-08 at 3.23.49 PM.png)

###Slot Prefab Details:
![](/assets/Screen Shot 2019-04-12 at 1.34.30 PM.png)
- **Slot: UI-Image GameObject**
    - Top Level: UI-Image, //sets slot's background color
        - Has Slot.cs script-component   
    

  - **2-Child GameObjects:**   
   - **UI-Image: SlotImage** 
        - Populate with test image, set color to Clear (transparent) 
    - **  UI-Text: CountText**
        - Shows number of items.  

###Slot: Inspector-panel Details
**Configure Slot GameObject, with Slot script-component** as shown below:

![](/assets/Screen Shot 2019-04-08 at 3.28.00 PM.png)


#Store-UI-SelectItems: 
**Create a UI Panel - with Children:**
- UI-Text - StoreTitle
- BtnAddDiamond 
 Buttons to Select PickUp-Items
 
![](/assets/Screen Shot 2019-04-12 at 2.22.09 PM.png)
![](/assets/Screen Shot 2019-04-12 at 2.28.03 PM.png)
![](/assets/Screen Shot 2019-04-12 at 2.49.28 PM.png)


###UI-Button Prefab to Add Items to Inventory

###BtnAddDiamond Prefab:
 - Included with InventoryDisplay Package

Configure Button OnClick to execute PickUp method:  AddItem( )
![](/assets/Screen Shot 2019-04-08 at 3.26.05 PM.png)


###PickUp-Item: UI-Image
Child of the BtnAddDiamond, 
Configured as shown below: with a **PickUp Script component,**  where the **ItemInstance: item** has been populated with a ScriptableObject of an Item child-class: (Gem, Potion ScriptableObjects included with [InventorySystem scripts](https://kdoore.gitbooks.io/cs-2335/content/project-2-dictionaries-to-store-data/inventory-scriptableobject/overview.html#unity-package-with-updated-code-files))

![](/assets/Screen Shot 2019-04-12 at 2.55.35 PM.png)





