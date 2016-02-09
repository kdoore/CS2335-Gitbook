#NPC Collection: 
##IEnumerable, IEnumerator Interfaces

###Enumeration:  *Let me count the ways*

[*Enumeration*](https://en.wikipedia.org/wiki/Enumeration) is commonly used in mathematics, computer science, and other fields: it refers to a listing of elements in a collection.  Depending on the context, this can have a variety of subtle distinctions, are there duplicate elements, are the elements ordered by some value?  In this current context:  C# Enumeration, we want to define interfaces for our custom collections so that we can step through the collection and examine each element, in some ordered way. 

###C# IEnumerable and IEnumerator Interfaaces 

We want to create our own collection class where we can have a collection of custom game objects like zombies.  This class will allow us to store a number of zombie-like objects. We will create a base-class: NPCharacters that will be the base-class for similar zombie-like classes. We can use a variety of different data structures such as an array or a list within our collection, to store our individual NPCharacter object instances, but we want our implementation choice of data structures to be hidden to enforce encapsulation for our class. In order to provide access to elements of our NPCharacter collection class, we want to implement the C# IEnumerable and the IEnumerator interfaces which allow us to use foreach to operate on each element of our collection.

###Foreach
[MSDN Reference:](https://msdn.microsoft.com/en-us/library/ttw7t8t6.aspx)   The foreach statement is used to iterate through a collection of elements.  It is similar to a for-loop except there is no index element used to identify each element in order to access it.  A C# class that represents a collection of elements can provide access to use the foreach statement with the collection class if the class implements both the IEnumerator and the IEnumerable Interfaces.  

###Encapsulation / Information Hiding
A class that implements IEnumerator and IEnumerable can enforce encapsulation / information-hiding because access can be provided to all collection elements without specifying the data-structure used within the class to hold the collection objects. 

###NPCharacter Collection
If our NPCharacter collection can be accessible by users of our class using the ``foreach`` method, then we can say that our collection is *Enumerable*, this means that we have implemented an *Enumerator*.  Since we are going to implement these interfaces, then we need to look at the interface specification so we understand what code we are required to implement to fulfill the interface contract.

###MSDN Reference:  [IEnumerable](https://msdn.microsoft.com/en-us/library/system.collections.ienumerable.aspx)

```java
Methods:   GetEnumerator()  //returns an Enumerator that iterates through a collection
```

###MSDN Reference:  [IEnumerator](https://msdn.microsoft.com/en-us/library/system.collections.ienumerator.aspx)

```java
Properties:  Current  // this gets the current element in the collection

Methods:  MoveNext()   //advance the inumerator to the next element in the collection

          Reset()   //set the enumerator to the first element in the collection
            ```
            
```

###NPCollection Class - A collection
In the code below, we define a custom collection class:  NPCollection, and we implement the IEnumerator and IEnumeration interfaces for this class, so in Example.cs, we can interate through our collection without knowing the type of dataStructure: array, list, dictionary, that is used to hold our collection elements since we'll be able to use forEach directly with our collection.

```

using UnityEngine;
using System.Collections;


public class NPCollection : IEnumerable, IEnumerator   {  // foreach

    private NPCharacter[] characters; //dataStructure to hold collection elements

	private int position = -1;  //used for IEnumerator to keep track of the current instance
 	
	public NPCollection(){  //constructor 

		characters = new NPCharacter[3]; ///base class array

		//polymorphism: 
		//1. we can store child-class objects in an array
		// of the base-class type
		// 2. data-type will be determined at run-time to determine which
		// methods are executed: base or child class
		
		characters[0] = new Zombie("NPC_Rob", Random.Range(10,15));
		characters[1] = new Kitten();
		characters[2] = new Zombie("NPC_White", Random.Range(10,15));
        
	}  //end of constructor
		
	///nuts and bolts are in IEnumerator
	
	//this is like the for loop equivalent of: characters[i]
	public object Current{
		get{
			return characters [position];
		}
	}
    
    //this is like the for loop equivalent of initialization (int i=0)
	
	public void Reset(){
		position = -1;
	}
    
    //this is like the for loop equivalent: ( i< characters.Length; i++)
	public bool MoveNext(){
		position++;
		return position < characters.Length;
	}
    
    //This is required for IEnumerable, we need to provide access to the IEnumerator
    
	public IEnumerator GetEnumerator(){
		return (IEnumerator)this;
	}


}

```
###Implementation Example:  
Create a Example1 class that can be attached to a gameObject like the main camera.  This code creates an instance of our NPCollection,  then we can use ``foreach`` to step through each element in the collection since we have implemented IEnumerable and IEnumerator in the NPCollection class.  This supports encapsulation because we don't need to know what type of dataStructure is used to hold our collection's elements.  

Polymorphism allows us to execute np.doSomething( ) which is defined as a virtual method in NPCharacter, and then we defined doSomething() in the Zombie class to explicitly over-ride the base-class method. 

```
using UnityEngine;
using System.Collections;

public class Example : MonoBehaviour {
	
	public NPCollection myCollection;
		
		// Use this for initialization
		void Start () {
		myCollection = new NPCollection ();

		foreach (NPCharacter npc in myCollection) {    //use foreach with collection  
			Debug.Log (npc.ToString ());   ///polymorphism determines overriding

			npc.doSomething();    ///polymorphism determines method overriding 

			//here we can check to see if our current npc element is a zombie or kitten

			Zombie z = npc as Zombie;  //try to cast as a zombie
			if(z != null){   //if successful at casting as a zombie
				Debug.Log("Only a Zombie can eat brains");
				z.EatBrains(5);//this only applies to zombie objects
			}

			Kitten k = npc as Kitten;
			if (k != null) {  //if successful at casting as a kitten
				Debug.Log ("Only a Kitten can drink milk");
					k.DrinkMilk (3);
            }
		}
			
		Debug.Log ("Number of Zombies" + Zombie.NumberOfZombies);
		Debug.Log ("Number of Kittens" + Kitten.NumberOfKittens);
	}
}


```