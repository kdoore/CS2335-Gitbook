# C# Data Structures and Collections

[MSDN Reference](https://msdn.microsoft.com/en-us/library/7y3x785f.aspx)

C# provides a large variety of special classes that are designed for us to create a collection where we can maintain, access, modify a series of data.  We've learned that Arrays are a basic data structure, a structure for storing data.  

###Array
Arrays have a fixed size, so if we declare an Array to hold 100 elements, if our program needs to store 101 elements, then we have a problem.  To solve this, the ArrayList class was created.  The ArrayList size can be changed dynamically, as the program is executing, so we could use an ArrayList to store data as part of some user-input, because we wouldn't need to limit the user's allowed number of input values, but we wouldn't know ahead of time how many elements would be needed.    When we declare an Array, we must specify the type of element that the Array will hold.  `Car[] myCars = new Car[5];` This array will be able to hold object references for Car objects.  If we try to add a Dog object to the Array, we'll get a compile-time error.   `myCars[0] = new Dog();  ` this will cause an error.  

###ArrayList
When ArrayLists were designed as an improvement over Arrays, they were designed to hold references to `Object`.  `Object` is a special Type, all classes are child classes of the `Object` class.  So, since ArrayList is designed to hold `Object` - type references, this would seem to be ideal.  
Here is an example of ArrayList:

```
        ArrayList myCarList = new ArrayList();
         Car myFirstCar = new Car(); //create a Car object, and object reference.
         
         myCarList.add(myFirstCar); //we add a Car object reference to the ArrayList.
         
         myCarList.add( new Dog()); //there is a problem, we can also add a Dog to the ArrayList.
         
         In order to use an element from the ArrayList, we need to do a type-cast to the Car type before we can use the element:
         
         Car myTempCar = (Car) myCarList[i];  //type-cast required.
```

One problem with ArrayLists is that they aren't **Type-Safe**, we can't be guaranteed that the object-references in the ArrayList actually refer to a specific Type of object. 

To resolve this issue, C# introduced Generics, and the Generic Collection Classes.  These collections are similar to Arrays in that we must specify the data-type of object-references that can be stored in the collection.  All Generic Collection classes use the syntax <T> or something similar, where T is a placeholder that indicates we must specify the Type of objects *(object-references)* that can be stored in the collection.

``List < T > myGenericList = new List< T >();``  //T is a Generic Type Placeholder

### Generic List < T >

```
using System.Collections.Generic;  //we need to add this additional directive
    
    List<Car> myCarList = new List<Car>();   Declare and initialize a List to hold Car object references.
    
    myCarList.add( new Car());  //add a car obj. reference
    Car tempCar = myCarList[0];    //access a car object and assign to a temp reference
 ```   
 
    
           