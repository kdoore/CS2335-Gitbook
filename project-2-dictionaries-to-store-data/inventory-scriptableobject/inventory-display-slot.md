#Inventory Display and Slot Classes

**Unity Package file with Prefab of UI Display GameObjects**
Link: [UI-DisplayPrefab_Package-S19](https://utdallas.box.com/v/UI-InventoryDisplay-S19)

##InventoryDisplay Prefab GameObject:

###Images Inventory Display and Slot Prefabs

- **PhysicalInventory** is a  UI-Panel GameObject 
- **Monobehaviour Components** on the PhysicalInventory:  
    - **Grid-Layout**
    - **InventoryDisplay.cs**
    - **Optional:  Animator Component** - For Animated UI
    

- **Slot: Image GameObject with 2-Child GameObjects:**   
 - UI-Image: SlotImage, UI-Text: CountText.
    - Slot(0) :UI-Image - Top Level, is SlotBackground  
        - SlotImage: UI-Image - Populate with test image, set color to Clear (transparent) 
        -CountText: UI-Text, shows number of items.  

![](/assets/Screen Shot 2019-04-08 at 3.14.31 PM.png)
###Slot Inspector - Slot Script
![](/assets/Screen Shot 2019-04-08 at 3.28.00 PM.png)

##Add GridLayout Component, Configure as below
![](/assets/Screen Shot 2019-04-08 at 3.23.49 PM.png)

###BtnAddDiamond Prefab:
 - Included with InventoryDisplay Package
 The PickUp-Item, which is a child of the 

Test Buttons - Button with PickUp-Item as a child
![](/assets/Screen Shot 2019-04-08 at 3.23.05 PM.png)


Configure Button OnClick to execute PickUp method:  AddItem( )
![](/assets/Screen Shot 2019-04-08 at 3.26.05 PM.png)

