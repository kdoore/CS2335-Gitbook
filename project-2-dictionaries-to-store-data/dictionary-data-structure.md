#Dictionary Data-Structure

Links:  
[MSDN: Dictionary](https://docs.microsoft.com/en-us/dotnet/api/system.collections.generic.dictionary-2?redirectedfrom=MSDN&view=netframework-4.7.2)

[Gitbook Definition and Examples](/dictionary.md)

[Unity Scripting Tutorial: Dictionary ](https://unity3d.com/learn/tutorials/modules/intermediate/scripting/lists-and-dictionaries)

[Unity TextAdventure Tutorial - Use Dictionary Inventory](https://unity3d.com/learn/tutorials/topics/scripting/preparing-use-item-dictionary)

[Using Arrays, Lists, ArrayLists, Dictionaries, HashTable in Unity](https://hub.packtpub.com/arrays-lists-dictionaries-unity-3d-game-development/)

###Dictionaries represent a collection of Key,Value pairs of data.

Dictionary<TKey,TValue>    Generic Dictionary - you specify the data-type of the key: Tkey, and value: TValue.

Dictionaries provide a data-structure for storing data that has 2 associated parts, which is very useful for data associated with games, such as inventory systems, character choice systems, weapon systems, etc.  

Unity does not show Dictionary elements in the inspector.

**Include as a directive at top of script:**
**using System.Collections.Generic;**

### GameData Inventory - Score Pickup-Item count
public Dictionary<PickupType, int> inventory = new Dictionary<PickupType, int>();

