#PlayerDataEventArgs
The following class is used to encapsulate the Player data that we'll want to publish to as event data when the PlayerData has been updated, this will effectively package the player data so we can send it as an event message argument when an event gets executed. We can also use this data when we want to save as persistent data in the filesystem, so it can be accessed during subsequent  gameplay episodes.

If we use the C# System defined class EventArgs as the base class for our PlayerDataEventArgs, then we can use this new class to create a dataObject that we can pass as a parameter for custom events, and it will also work for the default C# EventHandler datatype.


```java
using UnityEngine;
using System.Collections;
using System;

[Serializable]  
public class PlayerDataEventArgs : EventArgs {
public int totalScore;
public int health;
public int lives;
}


```

The [Serializable] attribute in the code above indicates that this class can be Serialized, this means it can be saved to external memory as binary data so it can be recreated with the correct object values when needed.

See [Saving Data](https://kdoore.gitbooks.io/cs-2335/content/saving_data_-_serialization.html)

[MSDN Documentation for Serialization](https://msdn.microsoft.com/en-us/library/mt656716.aspx)