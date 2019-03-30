#ScriptableObjects

Scriptable Objects are essentially 'MonoBehaviors' but are not structured to be used as a gameObject's component. : Scriptable Objects don't require being attached to a GameObject, doesn't need to be (can't be) associated with a scene, transform, etc. SO's do not get standard Unity 'callback methods' like Start( ) , Update( ) etc, which means they don't have a 'frame-based' lifecycle.  They can be 'serialized' (saved by the system) and viewed in the inspector. [Richard Fine - Video 9:45](https://www.youtube.com/watch?v=6vmRwLYWNRo)

###Patterns:(video 21:30)

- DataObjects and Tables - Example - When killing a goblin, what are the items that the object can drop? 
Data: DropObject, ChanceOfDropping

- Extendable Enums - Empty ScriptableObjects bound to Asset Files - Example: DamageTypes.  You can compare, test equality on an emtpy object, ie: use as Dictionary Keys.  Benefit is that it's easy to add new values without changing 'code'. 

 - Dual Deserialization - Supports JSON Utility for saving, loading data: 
  - Example:  Level Layout - Editor with Level Data such as Platform, Player, Coin, Enemy position data. 
  
  - Reload-Proof Singletons - ![](/assets/Screen Shot 2019-03-30 at 7.26.46 AM.png) Video location: 31:00
  
  - Delegate Objects:  Scriptable Objects can contain methods, ( not just data ) - Strategy Pattern

###Resources

[Unity Tutorial - Scriptable-Objects](https://unity3d.com/learn/tutorials/modules/beginner/live-training-archive/scriptable-objects)

[Architect your Code in Unity](https://unity3d.com/how-to/architect-with-scriptable-objects)

[ScriptableObjects- Richard Fine](https://www.youtube.com/watch?v=6vmRwLYWNRo)