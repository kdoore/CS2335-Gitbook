# Dictionary

[MSDN Reference](https://msdn.microsoft.com/en-us/library/xfhwa508.aspx)

A Dictionary represents a collection of Key / Value pairs.

Dictionary < TKey, TValue > //syntax to declare a new Dictionary object-reference.

The Key is used as a indexer to allow storage of a value that is associated with a unique Key.  Keys must be unique, there cannot be duplicate Keys in a dictionary, so we must use care when adding new elements to a dictionary object to ensure that the item's key does not already exist in the dictionary.  There are several methods that can be used to test for the presence of a specific key, these helper functions prevent runtime errors in our programs.

###Dictionary.Add()
```java
        Dictionary<string, int> inventory = new Dictionary<string, int>();
         inventory.Add("frogs", 2);
         inventory.Add("puppies", 3);
         inventory.Add("frogs", 3); //this will generate an error since the frog key already exists
         ```
         
###Dictionary.ContainsKey( )
There are many Dictionary methods to let us interact and modify Dictionary elements.  Before we can work with the Dictionary, for inserts or any other modification, we should always to check to see if the key, associated with our current item. Otherwise, we'll get a run-time error based on the existence in the dictionary. Similarly, if we want to update the value of a dictionary item, we must first check that the key exists. 

```java
        //correct approach - check for key before using Add() method
         if(inventory.ContainsKey("frogs")){  //if key exists, then update value
              //to update a value
            inventory["frogs"] = 3;   //bracket notation gives access to the value element
         }
         else{
            inventory.Add("frogs", 1);  //create a new dictionary element that is key/value pair
         }
         
         int numCows = inventory["cows"];  //this will generate an error since the key doesn't exist
         
         
         ```
  ###TryGetValue( TKey key, out  val)       
   We can also use the C# Dictionary function:  TryGetValue( )
 to both test to see whether a key is in the dictionary, and it will return the value in the variable declared with the variable modifier ```out```
  
MSDN Reference: [TryGetValue(TKey key, out  val )](https://msdn.microsoft.com/en-us/library/bb347013.aspx)
   
 
  ```
 ///using System.Collection.Generic;  
 
       Dictionary< string, int> myDictionary = new Dictionary<string, int>();
		myDictionary.Add("Zombie", 1);
        
        //get value from dictionary
        
        int numZombies = 0;
		if(myDictionary.TryGetValue("Zombie", out numZombies)){
			Debug.Log(" NumZombies " + numZombies);
		}
        else{
           Debug.Log("No zombies found");
        }
 
 ```
           
         
         
 ###foreach Loop
In order to iterate through all items in a dictionary, we can use a foreach loop which is a range-based for-loop as opposed to a count-based for-loop.  Since each inventory item is a key/value pair which is a complex dataType, we can use ``var`` as the variable type that will temporarily hold each item so we can manipulate the item. We can also use the KeyValuePair struct data-type if we declare the <T> type of each dictionary element. 
```

         foreach(var item in inventory){  //use var variable type
	      	int itemTotal=item.Value;
	      	string itemName=item.Key
	        Debug.Log( itemName + "  " + itemTotal);
	      }  
	      
	      foreach( KeyValuePair<string, int> item in inventory){  
	             Debug.Log( item.Key + "  " + item.Value);
	      }
	      
	      ```
	      