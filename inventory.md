# Game Inventory

Inventory Code is based on: Unity 5x Cookbook by Matt Smith PacktPub.  

Sprites are from [LostGarden](LostGarden.com): Daniel Cook.[Small World Graphics](http://www.lostgarden.com/search/label/free%20game%20graphics)

[Zip File with Sprite Assets:](https://utdallas.box.com/project3starterCode)

###Player Inventory
For the inventory, we're going to create a player that can interact with other gameObjects, as a means to interact with items in their inventory.  

The [Unity Script Reference Manual](http://docs.unity3d.com/ScriptReference/) provides documentation for how to create game-like behaviors as interactions between GameObjects.

###GameData
We can consider a variety of values that are associated with the player's experience while playing the game, these values can be simple items like player health, enemy health, or experience points.  We'll want access to this data throughout our application, so we need to create a centralized data structure so we can organize and provide access to a variety of game data.  The way data is structured is referred to the Data Model.  The organizational structure of data has a big impact on the ease of interaction with the data and ease of modifying the data model itself.  There are a wide variety of data structures that can be implemented for any application.  The most simple data structure consists of storing a piece of data in a simple variable: `int numStars=0`, We can also consider classes as data structures which can also provide methods to modify the data associated with an object instance. 

###C# Collection Classes
Arrays and Lists are common data structures, provided by most languages which allow storage of identical types of information, where objects can be seen as an instance of a particular type of data.  Data stored in an array or list is organized in a sequential order.  When searching Lists or Arrays, it is necessary to look at each sequential element until the matching element is found.  For large sets of data, this can be very inefficient.  Arrays and Lists also allow for duplicate valued elements.  Lists and Arrays provide methods which allow sorting if a comparator for elements has been provided, so Lists and Arrays work well for data with ordered relationship between elements. Dictionaries are another C# collection class which are useful for storing data where there's a key-type variable that can be associated with each data instance, and where there's typically not a sequential ordering relationship between the data elements. The Unity manual provides an example of how to use Dictionaries and Lists in the linked tutorial: [Unity Tutorial on Lists and Dictionaries](http://unity3d.com/learn/tutorials/modules/intermediate/scripting/lists-and-dictionaries)

###Dictionaries
We will use C# dictionaries and we'll want to define some additional classes like: item that can help standardize our inventory data model. Dictionaries are also refered to HashMaps in some languages.  Dictionaries work well for types of data where there is an association relationship between data elements. Game data is often a good fit for dictionary data structures. A Dictionary element consists of a pair of components:  key and value.  The key is used as an index which can have semantic meaning, and the value is the data we want to store, retrive and modify.  A simple example would be a dictionary that stored enemies, we would use the type of enemy as the key and the count as the value so we'd have: key ="zombies", value=5, key="monsters", value=4. Dictionaries use generic types, so we need to declare the data type for keys and values when we declare the dictionary: 

```
     Dictionary<string, int> enemies = new Dictionary<string, int>();
        
        enemies.add( "zombies", 5);
        enemies.add("monsters", 4);
        
```

We can imagine that for most inventory items we can define a name, maybe a corresponding image, possibly a type, where there may be different types of inventory items, we can imagine needing to distinguish between items that can have different types of values: discrete, boolean, continuously valued etc.  

