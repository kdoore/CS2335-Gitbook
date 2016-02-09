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
            
###Zombie's Base-Class: NPC
If we realize that we're likely to have other classes that represent similar objects to Zombies, it would then make sense for use to define a Base-Class for all similar types of objects.  We can consider that zombies might be part of a larger classification of non-player-characters (NPC) that we'd have in a game, we can imagine we'll have other NPC types in our program.  It will be helpful to be able to group these similar objects in a collection, and if we make all similar classes inherit from the same base-class this will make it easy to manage a group of these similar objects.  


###Non-Player-Character: Base-Class
Since we've already created a Zombie class, it will be relatively easy to determine the required class: instance-variables and class methods that we'd need for a base-class that would be a parent-class for Zombies and other NPCs.  We can actually remove code from the zombie class and place that code directly in the NPCharacter base class, then all similar child-classes will inherit the same base-class instance-fields and methods.  The code below defines the NPCharacter Class:

```java

using UnityEngine;
using System.Collections;

public class NPCharacter {

	protected int hitPoints;
	protected float healthPoints;
	protected string name;
	protected GameObject prefab;

	#region properties

	public string Name{  //property
		set {
			name = value;
		}
		get{
			return name;
		}
	}

	public int HitPoints{       //myZombie.HitPoints = 4;
		set{
			hitPoints = value;
			}
		get{
			return hitPoints;
		}
	}
	#endregion

	public NPCharacter( ){    ////we must define a default constructor 
		hitPoints=0;
		healthPoints = 0;
		name = "No Name";
		Debug.Log ("Default Constructor for NPC is called");
	}
	public NPCharacter(string n, int hp){    ////other constructor
		hitPoints=hp;
		healthPoints = 0;
		name = n;
		Debug.Log ("Constructor for NPC is called");
	}
		
	public override string ToString ()
	{
		return string.Format ("NPC Name: {0}, HitPoints {1}", name, hitPoints);
	}
}


```

###NPCollection Class - A collection
In the code below, we define a custom collection class:  NPCollection, and we implement the IEnumerator and IEnumeration interfaces for this class.

```

public class NPCollection : IEnumerable, IEnumerator  {  // foreach

	private NPCharacter[] characters; //dataStructure to hold collection elements

	private int position = -1;  //used for IEnumerator to keep track of the current instance
 	
	public NPCollection(){  //constructor 

		characters = new NPCharacter[3];

		//polymorphism: 
		//1. we can store child-class objects in an array
		// of the base-class type
		// 2. data-type will be determined at run-time to determine which
		// methods are executed: base or child class
		characters[0] = new Zombie("NPC_Rob", Random.Range(10,15));
		characters[1] = new Zombie("NPC_Stubbs", Random.Range(10,15));
		characters[2] = new Zombie("NPC_White", Random.Range(10,15));

		//other NPC child-class objects can be in our collection
		/////character[0] = new Kitten((names[i], Random.Range(10,15));

		Debug.Log ("NPC " + characters[0].ToString ());
	}  //end of constructor
		
	///nuts and bolts are in IEnumerator

	///IEnumerator Property, this refers to the current selected
	/// collection object

	public object Current{  //property
		get{
			return characters [position];
		}
	}
	/// <summary>
	/// Reset this the collection position index.
	/// </summary>
	public void Reset(){
		position = -1;
	}

	/// <summary>
	/// Moves the next.
	/// </summary>
	/// <returns><c>true</c>, position is still a valid index for the collection, <c>false</c> otherwise.</returns>
	public bool MoveNext(){
		position++;
		return position < characters.Length;
	}

	/// <summary>
	/// This is for IEnumerable Interface
	/// Gets the enumerator.
	/// </summary>
	/// <returns>The enumerator.</returns>
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
	/// <summary>
	/// declare instance of collection
	/// </summary>
	private NPCollection myCollection;

	// Use this for initialization
	void Start () {
		
		myCollection = new NPCollection ();

		//use foreach to iterate through the collection, no need to know the 
		//datastructure used to store the collection elements

		foreach (NPCharacter np in myCollection) {
			Debug.Log(np.ToString ());  //ToString( ) is defined as override in both np and zombie 
			np.doSomething ();  //which version of doSomething( ) will be executed?
			
			//np.TakeDamage ();  //can't call TakeDamage on NPCharacter
			
			Zombie z=new Zombie();
			if (np.GetType () == z.GetType()) {     //test to see if the run-time type is a Zombie
				Zombie tempZ = np as Zombie;   		//Type cast np to a Zombie object
				tempZ.TakeDamage (5);			//TakeDamage  can only apply to a Zombie object
			}
		}

		Debug.Log ("Number of Zombies" + Zombie.NumberOfZombies);
	}

}

```