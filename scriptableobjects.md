#ScriptableObjects

Scriptable Objects are essentially 'MonoBehaviors' but are not structured to be used as a gameObject's component. : Scriptable Objects don't require being attached to a GameObject, doesn't need to be (can't be) associated with a scene, transform, etc. SO's do not get standard Unity 'callback methods' like Start( ) , Update( ) etc, which means they don't have a 'frame-based' lifecycle.  They can be 'serialized' (saved by the system) and viewed in the inspector. [Richard Fine - Video 9:45](https://www.youtube.com/watch?v=6vmRwLYWNRo)


###Resources

[Unity Tutorial - Scriptable-Objects](https://unity3d.com/learn/tutorials/modules/beginner/live-training-archive/scriptable-objects)

[Architect your Code in Unity](https://unity3d.com/how-to/architect-with-scriptable-objects)

[ScriptableObjects- Richard Fine](https://www.youtube.com/watch?v=6vmRwLYWNRo)