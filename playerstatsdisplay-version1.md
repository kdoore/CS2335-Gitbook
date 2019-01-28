#PlayerStatsDisplay - Version 1

Player Stats Display will be our first use of UI-GameObjects.  

We'll Use 2 UI-Text elements to display the Score and Health values that are stored in the GameData  component.

Steps: 
1.  Add 1 UI-Text Element to the Hierarchy, name it:  Score
2.  This initial creation of a UI element actually causes creation of 3 gameObjects that are added to the Hierarchy:
    - Canvas - the parent GameObject for all UI elements
    - Text - used for displaying text 
    - EventSystem - manages UI event-handling - if UI events don't seem to be working, make sure that one of these components is in the Hierarchy.
    
    - When any additional UI elements are added to the Hierarchy, the Canvas will be the parent element.  
    
See the following Pages for more info on working with UI Elements: 
    
    