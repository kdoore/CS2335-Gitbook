###UI-Animation:  Create InventoryDisplay Animation

The InventoryDisplay.cs script contains animation logic so the InventoryDisplay prefab: ISimple_Inventory is 'off-screen' when a scene starts, but when the Tab key is pressed, then the prefab moves to it's 'on-screen' position.  

- **[InventoryDisplay: download, add to Scene, configure:](/project-2-dictionaries-to-store-data/inventory-scriptableobject/inventory-display-slot.md)** Once you have the ISimple_Inventory added to the Canvas in a scene, and you've added the InventoryDisplay.cs script to the prefab, and hit 'apply' to update the prefab, then follow the Animation steps below. 

**Animation Steps:**

 - **Step 1: Add Animator Component** to the ISimple_Inventory prefab.: The script InventoryDisplay.cs script has animation logic included, so the script does not need to be modified.
 
 ![](/assets/Screen Shot 2019-04-17 at 1.52.58 PM.png)
 
 - **Step 2: Open Animation Window:** Menu > Window > Animation (anchor the Animation tab as you desire. In these images, it has been anchored to the bottom panel)
 
 - **Step 3: Create new Animation Clip** With the ISimple_Inventory gameObject selected in the Hierarchy, in the Animation window, select the button in the middle pane to create an animation clip for this gameObject.
 
 ![](/assets/Screen Shot 2019-04-17 at 2.03.55 PM.png)
 
 Create First: 1 of 2 AnimationClips
 ![](/assets/Screen Shot 2019-04-17 at 2.04.54 PM.png)
 
 - **Step 4: Add Property - Anchored Position** In the lower-left panel for the Animation window, select the Add Property Button, select the AnchoredPosition by selecting the gray-plus sign to the right of that option, see image below:
 
 ![](/assets/Screen Shot 2019-04-17 at 2.09.28 PM.png)
 
 - **Step 5: Remove Property: Anchored Position.y ** Select the Anchored Position.y and right-click to select Remove Property, this will insure that only the horizontal position will be animated.  
 
 ![](/assets/Screen Shot 2019-04-18 at 7.03.10 AM.png)
 
 - **Step 6: Remove Keyframes at time: 1.00**
 The image below shows that right-clicking on the blue highlighted keyframes at time: 1.00, gives the pop-up window option to delete the final keyframes for this animation. This insures that only the initial position of the InventoryDisplay is used for each animation clip.
 
![](/assets/Screen Shot 2019-04-18 at 7.06.35 AM.png)

- **Step 7: Repeat steps 3-6** to create **HideInventory Animation Clip ** For the HideInventory Animation Clip, move the InventoryDisplay prefab to it's off-screen postion.x to set the keyframes at 0.0;

- **Step 8: Configure Animator Controller.** Open the Animator Window.  After both animation clips have been created, the Animator Component now shows an Animator controller has been created: ISimple_Inventory, and set in the Animator Component.  See Image below.  The image below shows that InventoryHide should be the 'Layer Default State',it should be orange, with a connecting arrow from the Entry state. If this is not the case, right click on the InventoryHide state and select: **Set as Layer Default State**.

From each State node, right click to create a transition arrow to the other state.  
 
 ![](/assets/Screen Shot 2019-04-19 at 7.11.36 AM.png)
 
 - **Step 9:  Create Parameter:  Bool: IsVisible**
 In the left panel of the Animator Controller, create a Parameter: Bool 'IsVisible'.  Then select each transition arrow and configure the following logic in the right side panel: 
 
 Un-check: HasExitTime 
 InventoryHide -> InventoryShow if IsVisible is true
 InventoryShow -> InventoryHide if IsVisible is false
 
 - **Step 10: Test: Play the Scene, Press the 'Tab'** key to switch between showing / hiding the InventoryDisplay Prefab. 
 
 **Troubleshooting: ** 
 1. Make sure that the Parameter: **IsVisible** is spelled correctly, it must match spelling / capitalization used within the InventoryDisplay.cs script.
 2. Make sure the **InventoryDisplay.cs** script has been added as a component to the ISimple_Inventory Prefab.
 
 
 
 