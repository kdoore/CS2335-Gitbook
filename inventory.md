# Game Inventory

Inventory Code is based on: Unity 5x Cookbook by Matt Smith PacktPub.  

Sprites are from [LostGarden](LostGarden.com): Daniel Cook.[Small World Graphics](http://www.lostgarden.com/search/label/free%20game%20graphics)

[Zip File with Sprite Assets:](https://utdallas.box.com/s/f8q6fgq67v7r097zr49tk0lzrxrewj63)

###Player Inventory
For the inventory, we're going to create a player that can interact with other gameObjects, as a means to interact with items in their inventory.  

The [Unity Script Reference Manual](http://docs.unity3d.com/ScriptReference/) provides documentation for how to create game-like behaviors as interactions between GameObjects.

###GameDataModel
We can consider a variety of values that are associated with the player's experience while playing the game, these values can be simple items like playerHealth, enemyHealth.  Since we'll want access to this data throughout our application, then it makes sense to create a centralized data class so we can organize and provide access to a variety of game data.  The way data is structured is referred to the Data Model.  The organizational structure of data has a big impact on the ease of interaction with the data.  There are a wide variety of data structures that can be implemented for any application.  The most simple data structure consists of storing a piece of data in a simple variable: `int numStars=0`.  Arrays and Lists are common data structures which allow storage of identical types of information, where objects can be seen as an instance of a particular type of data.   