#Inventory Display Prefabs 
The Inventory-System's user-interface uses several prefab UI gameObjects that are included in the download link below.  Additional configuration steps are required to complete the InventoryDisplay functionality, instructions are provided along with images below.

 - **Step 1: Important, Make sure that the Inventory-System code has been installed prior to installing the InventoryDisplay**, because the InventoryDisplay requires class code in the InventorySystem code files. 

After downloading and installing all **[InventorySystem](/project-2-dictionaries-to-store-data/inventory-scriptableobject.md)** class files. Fix any errors / issues caused by downloading the InventorySystem files. Then proceed with download / install of the InventoryDisplay UnityPackage file.  Follow steps in **[InventorySystem](/project-2-dictionaries-to-store-data/inventory-scriptableobject.md)**  gitbook page to resolve issues. 

-  **Step 2: Download Unity Package file with Prefabs** of UI Inventory Display GameObjects
**Link:** [UI-DisplayPrefab_Package-S19](https://utdallas.box.com/v/UI-InventoryDisplay-S19)

##InventoryDisplay, Slot Prefabs: Installation Details:

###Inventory Display Prefab - ISimpleInventory:
The image below shows the hierarchy, inspector panels after the ISimpleInventory Prefab is added to the Canvas.

- **ISimpleInventory** is a  UI-Panel GameObject 
    - **Monobehaviour Components** on the **PhysicalInventory:**  
        - **Grid-Layout**
        - **InventoryDisplay.cs**
        - **Optional:  Animator Component** - For Animated UI
- **4- Slot Prefabs as Children**
    
** GridLayout Component, configuration shown below:**

![](/assets/Screen Shot 2019-04-13 at 11.43.15 AM.png)

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
**Slot GameObject, with Slot script-component configuration** as shown below: 

![](/assets/Screen Shot 2019-04-08 at 3.28.00 PM.png)


#Store-UI-SelectItems:
The Store-UI-SelectItems Prefab must be created by each student.  The images below show details of the GameObject hierarchy and inspector panel of the top-level UI-Panel.

The Store is a UI-Panel gameObject that has children gameObjects that are BtnAdd-Item objects.  The BtnAddDiamond prefab was included in the package file.  


![](/assets/Screen Shot 2019-04-12 at 2.22.09 PM.png)

![](/assets/Screen Shot 2019-04-12 at 2.42.24 PM.png)

- **Step 1: Create a UI Panel:** Store-UI-SelectItems (with Children). Adjust: Rect-Transform to determine size, anchors to fit your project. 
 - Component: Horizontal LayoutGroup (see image below to configure)
 
 
![](/assets/Screen Shot 2019-04-12 at 2.49.28 PM.png)

**Child GameObjects:**
- UI-Text: StoreTitle
- BtnAddDiamond - PreFab provides button-click event to select PickUp-Items, to be added to the Inventory-System. (see details below)

###BtnAddDiamond Prefab:  

**UI-Button Prefab to Add Items to Inventory**

 - **Included with InventoryDisplay Package** [Linked at top of this document](/[UI-DisplayPrefab_Package-S19](https://utdallas.box.com/v/UI-InventoryDisplay-S19))
 
 - Create additional **BtnAdd - Items** by creating a duplicate of the BtnAddDiamond prefab in the hierarchy to match your game's theme. 
 
 - Note the Configuration of the Button (script)'s OnClick() to execute PickUp method:  AddItem( ) as displayed in the inspector below.
 
![](/assets/Screen Shot 2019-04-08 at 3.26.05 PM.png)


###PickUp-Item:  UI-Image with PickUp script.

- Child of the BtnAddDiamond
- Component: PickUp Script 

Configured as shown below: with a **PickUp Script-component,**  

The **ItemInstance: item ** has been populated with a ScriptableObject of an Item child-class: (Gem, Potion ScriptableObjects included with  ( [InventorySystem scripts](https://kdoore.gitbooks.io/cs-2335/content/project-2-dictionaries-to-store-data/inventory-scriptableobject/overview.html#unity-package-with-updated-code-files))

![](/assets/Screen Shot 2019-04-12 at 2.55.35 PM.png)





