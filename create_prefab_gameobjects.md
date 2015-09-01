![](nXkq9arLMv.gif)
The animation above shows how to create a cube game object.  Next the cube is dragged into the Project Assets Panel, this turns the cube game object into a PreFab Game Object.  

We are then able to drag multiple cubePrefab objects into the scene.  We can add a redMaterial to one cubePrefab, then when we select Apply in the inspector panel for the prefab object, it applies this material to all prefab instances.  

We can individually modify a prefab by making adding a green material to one CubePrefab instance and then not applying that change to all of the other prefabs.

Next, in order to create a compound object, we create an empty gameObject.  We name it 'wall', then select each of the cubePrefab objects in the hierarchy panel and dragging them on-top of the 'wall' item in the hierarchy panel.  This causes these cubePrefab objects to be come child objects of the wall object.  This means that the transform object for each of these prefabs is now defined relative to the parent `wall` object.  If we select the 