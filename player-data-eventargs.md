#PlayerDataEventArgs
The following class is used to encapsulate the Player data that we'll want to publish to as event data when the PlayerData has been updated, this will effectively package the player data so we can send it as an event message argument when an event gets executed. We can also use this data when we want to save as persistent data in the filesystem, so it can be accessed during subsequent  gameplay episodes.

If we use the C# System defined class EventArgs as the base class for our PlayerDataEventArgs, then we can use this new class to create a dataObject that we can pass as a parameter for custom events, and it will also work for the default C# EventHandler datatype.


```
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