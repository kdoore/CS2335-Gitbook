###Dialog and Scene-Decision Integration

###How to Integrate Dialog with Scene-Transition logic:
In order for this to work as a prefab, we have written the code in a general way so it'll work in any scene. In other words,  we don't want scene-specific logic in any of the code for this script. 

In addition, we want all scene-transition logic to be contained in the StateX.cs files, so is it possible to have dialog logic combined with the buttons for scene-transition decisions? So, how do we get the dialog panel (prefab) to include a panel that has buttons with logic to leave the scene?

- **Issue: we want the general behavior of a prefab, but we want to integrate scene-transition decision logic. ** 
  
  Options:  
    
    1.  Don't use a prefab that includes the attached custom script logic.  
    2.  Write a custom script every-time we want to display dialog
    3.  Put some logic into a static utility function that can be accessed anywhere
    4.  Add logic to the current prefab script to open a new panel when there are no remaining dialog items.  
    5.  Create custom events to allow other script components to be notified when the next button is clicked but there is no remaining dialog to be displayed.   
       





