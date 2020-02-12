# C# Data Structures and Collections

[MSDN Reference](https://msdn.microsoft.com/en-us/library/7y3x785f.aspx)

C# provides a large variety of special classes that are designed for us to create a collection where we can maintain, access, modify a series of data.  We've learned that Arrays are a basic data structure, a structure for storing data.  

###Array
Arrays have a fixed size, so if we declare an Array to hold 100 elements, if our program needs to store 101 elements, then we have a problem.  When we declare an Array, we must specify the type of element that the Array will hold.  `Car[] myCars = new Car[5];` This array will be able to hold object references for Car objects.  If we try to add a Dog object to the Array, we'll get a compile-time error.   `myCars[0] = new Dog();  ` this will cause an error.   

###ArrayList
ArrayLists were designed as an improvement over Arrays, ArrayLists were designed to hold references to `Object`.  `Object` is a special Type, all classes are child classes of the `Object` class.  So, since ArrayList is designed to hold `Object` - type references, this would seem to be ideal.  The ArrayList size can be changed dynamically, as the program is executing, we can add or remove elements from an ArrayList at anytime using a variety of ArrayList methods such as: Add( Object o).
However, one problem with ArrayLists is that they aren't **Type-Safe**, we can't be guaranteed that the object-references in the ArrayList actually refer to a specific Type of object. In order to use an element from an ArrayList, we must do a Type-cast conversion to insure the element is compatible with the object-type that we are expecting. So this adds an additional step that must be done whenever we try to use an element we have stored in an ArrayList.  In addition, no error will occur when adding of object to an ArrayList, so we can't be sure someone on our programming team didn't accidentally add an incorrect object Type to our ArrayList.  For this reason, ArrayList is not considered _Type-safe_.

Here is an example of ArrayList:

```
        ArrayList myCarList = new ArrayList();
         Car myFirstCar = new Car(); //create a Car object, and object reference.
         
         myCarList.add(myFirstCar); //we add a Car object reference to the ArrayList.
         
         myCarList.add( new Dog()); //there is a problem, we can also add a Dog to the ArrayList.
         
         In order to use an element from the ArrayList, we need to do a type-cast to the Car type before we can use the element:
         
         Car myTempCar = (Car) myCarList[i];  //type-cast required.
```

###C# Generics
To resolve this issue of _Type-Safety_, C# introduced Generics, and the Generic Collection Classes.  These collections are similar to Arrays in that we must specify the data-type of object-references that can be stored in the collection.  All Generic Collection classes use the syntax <T> or something similar, where T is a placeholder that indicates we must specify the Type of objects *(object-references)* that can be stored in the collection.

``List < T > myGenericList = new List< T >();``  //T is a Generic Type Placeholder

###Generic List < T >
[MSDN Reference: List< T >](https://msdn.microsoft.com/en-us/library/6sh2ey19.aspx)

In order to use Generic Collections like List< T >, we must include a directive: _using System.Collections.Generic;_ at the top of our file. In order to use a Generic Type, we simply specify the data-type of the element that we will use in the collection as seen below.  The actual items `stored` in the collection are object references which hold the memory address of an associated object.
List Example: 
```
using System.Collections.Generic;  //we need to add this additional directive
    
    List<Car> myCarList = new List<Car>();   Declare and initialize a List to hold Car object references.
    
    myCarList.add( new Car());  //add a car obj. reference
    Car tempCar = myCarList[0];    //access a car object and assign to a temp reference
 ```   
 Lists 
 
 ###[Dictionary < TKey >< TValue >](https://kdoore.gitbooks.io/cs-2335/content/dictionary.html#dictionary)
 
 A Dictionary is a Type-Safe data-structure designed to hold data that has an association between a lookup-key and a value we'd like to store using that key.  This is different than a list because a list simply stores data in an ordered arrangement.  Dictionaries use a hash-algorithm which makes searching for the key an efficient process compared to search implemented for a List or Array.  Dictionaries cannot have duplicate keys. To create a Dictionary, you must specify the Type for both the Key and the Value.  A foreach loop is used to iterate through all elements of a dictionary.  Using a dictionary requries testing to determine if a Key already exists within a dictionary, before trying to add a key-value pair.  If the dictionary already contains a matching key, then attempting to add a duplicate key will cause an error.  To retrieve a value from a Dictionary, first check to see if the key exists within the dictionary, then use the key with bracket notation to retrieve the associated value: 
 ```
 ///using System.Collection.Generic;  
 
 Dictionary< string, int> myDictionary = new Dictionary<string, int>();
		myDictionary.Add("Zombie", 1);
		if(myDictionary.ContainsKey("Zombie")){
			int numZombies = myDictionary["Zombie"];
		}
        
        //we'll get an error if we try to add another item with the same key
        
        myDictionary.Add("Zombie", 2); ///ERROR
 
 ```
 We can also use the C# Dictionary function:  TryGetValue( )
 to both test to see whether a key is in the dictionary, and it will return the value in the variable declared with the variable modifier ```out```
  
 ### MSDN Reference: [TryGetValue( )](https://msdn.microsoft.com/en-us/library/bb347013.aspx)
  
   
 
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
    
           