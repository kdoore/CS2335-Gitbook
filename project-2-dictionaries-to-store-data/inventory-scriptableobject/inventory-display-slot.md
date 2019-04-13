#Prefabs: Inventory Display, Slot, Store, BtnAdd-Item 
The User-Interface uses several Prefabs included in the link below.  Additional configuration steps are required to complete the InventoryDisplay functionality.

After downloading and installing all **InventorySystem** class files in the previous section, and fixing all errors caused by changes to over-written files, then download the following UnityPackage file.

**Download: Unity Package file** with Prefab of UI Inventory Display GameObjects
**Link:** [UI-DisplayPrefab_Package-S19](https://utdallas.box.com/v/UI-InventoryDisplay-S19)

##InventoryDisplay and Slot Prefab GameObjects:

###Inventory Display Prefab - PhysicalInventory:
![](/assets/Screen Shot 2019-04-08 at 3.14.31 PM.png)

- **PhysicalInventory** is a  UI-Panel GameObject 
    - **Monobehaviour Components** on the **PhysicalInventory:**  
        - **Grid-Layout**
        - **InventoryDisplay.cs**
        - **Optional:  Animator Component** - For Animated UI
- **4- Slot Prefabs as Children**
    
** Configure GridLayout Component as shown below:**
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
![](/assets/Screen Shot 2019-04-12 at 2.22.09 PM.png)

![](/assets/Screen Shot 2019-04-12 at 2.42.24 PM.png)

**Create a UI Panel:** Store-UI-SelectItems (with Children)
 - Component: Horizontal LayoutGroup (see image below to configure)
 
 
![](/assets/Screen Shot 2019-04-12 at 2.49.28 PM.png)

**Child GameObjects:**
- UI-Text: StoreTitle
- BtnAddDiamond - PreFab, to Select PickUp-Items (see details below)

###BtnAddDiamond Prefab:  

**UI-Button Prefab to Add Items to Inventory**

 - **Included with InventoryDisplay Package** [Linked at top of this document](/[UI-DisplayPrefab_Package-S19](https://utdallas.box.com/v/UI-InventoryDisplay-S19))
 
 - Create additional **BtnAdd - Items** by creating a duplicate of the BtnAddDiamond prefab in the hierarchy to match your game's theme. 
 
 - Note the Configuration of the Button (script)'s OnClick() to execute PickUp method:  AddItem( ) as displayed in the inspector below.
 
![](/assets/Screen Shot 2019-04-08 at 3.26.05 PM.png)


###PickUp-Item: UI-Image

- Child of the BtnAddDiamond
- Component: PickUp Script 

Configured as shown below: with a **PickUp Script-component,**  

The **ItemInstance: item ** has been populated with a ScriptableObject of an Item child-class: (Gem, Potion ScriptableObjects included with [InventorySystem scripts](https://kdoore.gitbooks.io/cs-2335/content/project-2-dictionaries-to-store-data/inventory-scriptableobject/overview.html#unity-package-with-updated-code-files))

![](/assets/Screen Shot 2019-04-12 at 2.55.35 PM.png)





