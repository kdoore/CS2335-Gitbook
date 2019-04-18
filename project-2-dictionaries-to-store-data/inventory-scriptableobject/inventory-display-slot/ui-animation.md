###UI-Animation:  InventoryDisplay Prefab

The InventoryDisplay.cs script contains animation logic so the InventoryDisplay prefab: ISimple_Inventory is 'off-screen' when a scene starts, but when the Tab key is pressed, then the prefab moves to it's 'on-screen' position.  

- **[InventoryDisplay: download, add to Scene, configure:](/project-2-dictionaries-to-store-data/inventory-scriptableobject/inventory-display-slot.md)** Once you have the ISimple_Inventory added to the Canvas in a scene, and you've added the InventoryDisplay.cs script to the prefab, and hit 'apply'.  Follow the Animation steps below. 

**Animation Steps:**

 - **Step 1: Add Animator Component** to the ISimple_Inventory prefab.: The script InventoryDisplay.cs script has animation logic included, so the script does not need to be modified.
 
 ![](/assets/Screen Shot 2019-04-17 at 1.52.58 PM.png)
 
 - **Step 2: Open Animation Window:** Menu > Window > Animation (anchor the Animation tab as you desire. In these images, it has been anchored to the bottom panel)
 
 - **Step 3: Create new Animation Clip** With the ISimple_Inventory gameObject selected in the Hierarchy, in the Animation window, select the button in the middle pane to create an animation clip for this gameObject.
 
 ![](/assets/Screen Shot 2019-04-17 at 2.03.55 PM.png)
 
 Create First: 1 of 2 AnimationClips
 ![](/assets/Screen Shot 2019-04-17 at 2.04.54 PM.png)
 
 
 
 - **Step 4: Add Property** In the lower-left panel for the Animation window, select the Add Property Button, select the AnchoredPosition by selecting the gray-plus sign to the right of that option, see image below:
 
 ![](/assets/Screen Shot 2019-04-17 at 2.09.28 PM.png)
 
 