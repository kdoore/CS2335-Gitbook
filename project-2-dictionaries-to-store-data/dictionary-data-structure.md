#Dictionary Data-Structure



Links:  
[MSDN: Dictionary](https://docs.microsoft.com/en-us/dotnet/api/system.collections.generic.dictionary-2?redirectedfrom=MSDN&view=netframework-4.7.2)

[Gitbook Definition and Examples](/dictionary.md)

[Unity Scripting Tutorial: Dictionary ](https://unity3d.com/learn/tutorials/modules/intermediate/scripting/lists-and-dictionaries)

[Unity TextAdventure Tutorial - Use Dictionary Inventory](https://unity3d.com/learn/tutorials/topics/scripting/preparing-use-item-dictionary)

[Using Arrays, Lists, ArrayLists, Dictionaries, HashTable in Unity](https://hub.packtpub.com/arrays-lists-dictionaries-unity-3d-game-development/)

###Dictionaries: A collection of Key,Value pairs
Dictionaries provide a data-structure for storing data that has 2 associated parts, which is very useful for data associated with games, such as inventory systems, character choice systems, weapon systems, etc.  


The Key is used as a index to allow storage of a value that is associated with a unique Key. **Keys must be unique, there cannot be duplicate Keys** in a dictionary, so we must use care when adding new elements to a dictionary object to ensure that the item's key does not already exist in the dictionary. There are several methods that can be used to test for the presence of a specific key, these helper functions prevent runtime errors in our programs.


Unity does not show Dictionary elements in the inspector.

**Include as a directive at top of script:**
**using System.Collections.Generic;**

Dictionary < TKey,TValue >    
Dictionary - you specify the data-type of the key: Tkey, and value: TValue.


Unity does not show Dictionary elements in the inspector.

### Example: GameData Inventory 
Stores: Pickup-Item count

```java
public Dictionary<PickupType, int> inventory = new Dictionary<PickupType, int>(); //initialize

inventory.Add(PickupType.Gem, 1); //add an item
int gems = inventory[PickupType.Gem]; //get value associated with key

inventory[PickupType.Gem] = 2; //change an item

if( inventory.ContainsKey( PickupType.Cow ){
  int count = inventory[PickupType.Cow]
  inventory[PickupType.Cow] = count +1;
}else{
   Debug.Log("No Cows")
   inventory.Add( PickupType.Cow, 1); //added
}

//TryGetValue
int count = 0;
if( inventory.TryGetValue( PickupType.Gem,out count){
  inventory[PickupType.Gem] = count + 1;
  } 
  else{
    inventory.Add(PickupType.Gem ,1);
  }

```


