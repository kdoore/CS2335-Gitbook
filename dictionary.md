# Dictionary

[MSDN Reference](https://msdn.microsoft.com/en-us/library/xfhwa508(v=vs.110).aspx)

A Dictionary represents a collection of Key / Value pairs.

The Key is used as a indexer to allow storage of a value that is associated with a unique Key.  Keys must be unique, there cannot be duplicate Keys in a dictionary, so we must use care when adding elements to a dictionary object to ensure that the item's key does not already exist in the dictionary.  There are several methods that can be used to test for the presence of a specific key, these helper functions prevent runtime errors in our programs.




Foreach Loop.  In order to iterate through all items in a dictionary, we can use a foreach loop.  Since each inventory item is a key/value pair which is a complex dataType, we can use var as the variable type that will hold a 

```
        Dictionary<string, int> inventory = new Dictionary<string, int>();
         inventory.Add("frogs", 2);
         inventory.Add("puppies", 3);
         inventory.Add("frogs", 3); //this will generate an error since the frog key already exists
         
         //to update a value
         inventory["frogs"] = 3;   //bracket notation
         int numCows = inventory["cows"];  //this will generate an error since the key doesn't exist

         foreach(var item in inventory){  //use var variable type
	      	int itemTotal=item.Value;
	      	string itemName=item.Key
	        Debug.Log( itemName + "  " + itemTotal);
	      }  
	      
	      foreach( KeyValuePair<string, int> item in inventory){  
	             Debug.Log( item.Key + "  " + item.Value);
	      }
	      
	      ```
	      