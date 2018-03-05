#Dialog and Scene-Transition Integration

###How to Integrate Dialog with Scene-Transition logic:
In order for this to work as a prefab, we have written the code in a general way so it'll work in any scene. In other words,  we don't want any scene-specific logic in any of the code for this script. 

In addition, we want all scene-transition logic to be contained in the StateX.cs files. So how can we integrate dialog display with the buttons for scene-transition decisions? How do we get the dialog panel (prefab) to include a final display state, with buttons that can be used for scene transition?

- **Issue: we want the general behavior of a prefab, but we want to integrate scene-transition decision logic. ** 

There are a few different issues here:
 
  - **Issues:**

   - We don't want to add scene specific logic to a prefab, otherwise we can't use it anywhere. 
   - We want all scene-transition logic in the StateX.cs files, not attached to a panel gameObject
   - We don't have a way to let the StateX file know that the dialog has finished, so that scene-transition buttons can be displayed after the dialog has finished.
  
-  **Options:  **
    
    1.  Don't use a prefab that includes the attached custom script logic.
    2.  Have the scene-transition buttons hidden beneath the dialog panel, so they are viewable after the dialog panel is hidden.
    3.  Write a custom script every-time we want to display dialog
    4.  Put some logic into a static utility function that can be accessed anywhere
    5.  Add logic to the current prefab script to open a new panel when there are no remaining dialog items, have decision logic associated with the newly opened panel.  
    6.  Create custom events to allow other script components to be notified when the next button is clicked but there is no remaining dialog to be displayed.   (we'll learn how to create and use custom-events later in the semester)
       
###Option 4 - Add Logic to Open Another Panel




