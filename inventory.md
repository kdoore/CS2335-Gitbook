# Game Inventory

Inventory Code is based on: Unity 5x Cookbook by Matt Smith PacktPub.  

Sprites are from [LostGarden](LostGarden.com): Daniel Cook.[Small World Graphics](http://www.lostgarden.com/search/label/free%20game%20graphics)

[Zip File with Sprite Assets:](https://utdallas.box.com/s/f8q6fgq67v7r097zr49tk0lzrxrewj63)

###Player Inventory
For the inventory, we're going to create a player that can interact with other gameObjects, as a means to interact with items in their inventory.  

The [Unity Script Reference Manual](http://docs.unity3d.com/ScriptReference/) provides documentation for how to create game-like behaviors as interactions between GameObjects.

###GameData
We can consider a variety of values that are associated with the player's experience while playing the game, these values can be simple items like player health, enemy health, or experience points.  We'll want access to this data throughout our application, so we need to create a centralized data structure so we can organize and provide access to a variety of game data.  The way data is structured is referred to the Data Model.  The organizational structure of data has a big impact on the ease of interaction with the data and ease of modifying the data model itself.  There are a wide variety of data structures that can be implemented for any application.  The most simple data structure consists of storing a piece of data in a simple variable: `int numStars=0`, We can also consider classes as data structures which can also provide methods to modify the data associated with an object instance. 

###C# Collection Classes
Arrays and Lists are common data structures, provided by most languages which allow storage of identical types of information, where objects can be seen as an instance of a particular type of data.  Data stored in an array or list is organized in a sequential order.  When searching Lists or Arrays, it is necessary to look at each sequential element until the matching element is found.  For large sets of data, this can be very inefficient.  Arrays and Lists also allow for duplicate valued elements.  

###Dictionaries
We will use C# dictionaries and we'll want to define some additional classes like: item that can help standardize our inventory data model.  We can imagine that for most inventory items we can define a name, a corresponding image, possibly a type, where there may be different types of inventory items, we can imagine needing to distinguish between items that can have different types of values: discrete, boolean, continuously valued etc.   