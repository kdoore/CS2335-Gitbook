#Inventory Display and Slot Classes


##Hierarchy and Inspector Panel 
###Images for Slot and Inventory Display

- Physical Inventory is a UI Panel GameObject
- Slot GameObject has 3 elements:  2 UI-Images, 1 UI-Text
    - Slot(0) :UI-Image - Top Level, is SlotBackground  
        - SlotImage: UI-Image - Populate with test image, set color to Clear (transparent) 
        -CountText: UI-Text, shows number of items.  

![](/assets/Screen Shot 2019-04-08 at 3.14.31 PM.png)
###Slot Inspector - Slot Script
![](/assets/Screen Shot 2019-04-08 at 3.28.00 PM.png)

##Add GridLayout Component, Configure as below
![](/assets/Screen Shot 2019-04-08 at 3.23.49 PM.png)

Test Buttons - Button with PickUp-Item as a child
![](/assets/Screen Shot 2019-04-08 at 3.23.05 PM.png)


Configure Button OnClick to execute PickUp method:  AddItem( )
![](/assets/Screen Shot 2019-04-08 at 3.26.05 PM.png)

