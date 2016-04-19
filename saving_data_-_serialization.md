# Saving Data - Serialization

MSDN Reference Example: [Binary Serialization](https://msdn.microsoft.com/en-us/library/4abbf6k0%28v=vs.110%29.aspx)

"Serialization can be defined as the process of storing the state of an object to a storage medium. During this process, the public and private fields of the object and the name of the class, including the assembly containing the class, are converted to a stream of bytes, which is then written to a data stream. When the object is subsequently deserialized, an exact clone of the original object is created."

"Why would you want to use serialization? The two most important reasons are to persist the state of an object to a storage medium so an exact copy can be re-created at a later stage, and to send the object by value from one application domain to another. "  [ MSDN Serialization Concepts ](https://msdn.microsoft.com/en-us/library/39x7ad2x.aspx)

Script Serialization in Unity - [Reference Manual](http://docs.unity3d.com/Manual/script-Serialization.html)

In Unity, objects that are marked with the Serialized Attribute, or fields marked with SerializeField Attribute, are persisted by the Unity serialization process.  This serialization allows for saving of data between the unity editor system, where objects are created using C# or UnityScript, for design-time use, and the Unity game-engine for game-play mode, which uses C++ S However there may be cases when we want more customized control over serialization of game data.

[Unity SerializeField Reference](http://docs.unity3d.com/ScriptReference/SerializeField.html)

[Tutorial on Saving - Loading Game Data - TutsPlus](http://gamedevelopment.tutsplus.com/tutorials/how-to-save-and-load-your-players-progress-in-unity--cms-20934)

[Tutorial on Saving - Loading Game Data - SitePoint](http://www.sitepoint.com/saving-and-loading-player-game-data-in-unity/)