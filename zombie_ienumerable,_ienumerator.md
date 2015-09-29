#Zombies: IEnumerable, IEnumerator

###Enumeration:  *Let me count the ways*

[*Enumeration*](https://en.wikipedia.org/wiki/Enumeration) is commonly used in mathematics, computer science, and other fields: it refers to a listing of elements in a collection.  Depending on the context, this can have a variety of subtle distinctions, are there duplicate elements, are the elements ordered by some value?  In this current context:  C# Enumeration, we want to define interfaces for our custom collections so that we can step through the collection and examine each element, in some ordered way. 

The IEnumerable and IEnumerator interfaaces 

We want to create our own collection class:  Zombies.  This class will allow us to store a number of zombie objects.  We can use a variety of different data structures such as an array or a list within our collection, to store our individual zombie instances, but we want our implementation choice of data structures to be hidden to enforce encapsulation for our class. In order to provide access to elements of our Zombies collection, we want to implement the C# IEnumerable and the IEnumerator interfaces which allow us to use foreach to operate on each element of our collection.

If our Zombies collection can be accessable by users of our class using the foreach method, then we can say that our collection is *Enumerable*, this means that we have implemented an *Enumerator*.  Since we are going to implement these interfaces, then we need to look at the interface specification so we understand what code we are required to implement to fulfill the interface contract.

MSDN Reference:  [IEnumerable](https://msdn.microsoft.com/en-us/library/system.collections.ienumerable.aspx)

```java
Methods:   GetEnumerator()  //returns an Enumerator that iterates through a collection
```

MSDN Reference:  [IEnumerator](https://msdn.microsoft.com/en-us/library/system.collections.ienumerator.aspx)

```java
Properties:  Current  // this gets the current element in the collection

Methods:  MoveNext()   //advance the inumerator to the next element in the collection

          Reset()   //set the enumerator to the first element in the collection
            ```
###Zombies
In the code below, we define a new collection class:  Zombies, and we implement the IEnumerator and IEnumeration interfaces for this class.


public class Zombies: IEnumerable, IEnumerator {
	
	private Zombie[] zombieArray;  //we're using an array to store our collection of zombie objects

	//
	private int position = -1;
	
	//constructor
	public Zombies(){  
		//initialize our array with zombie objects
		zombieArray = new Zombie[3];
		zombieArray[0]=new Zombie("Stubbs", Random.Range (10,15));
		zombieArray[1]=new Zombie("Rob", Random.Range (10,15));
		zombieArray[2]=new Zombie("White", Random.Range (10,15));
	}
	
	//IEnumerable  //method that returns the enumerator
	public IEnumerator GetEnumerator(){
			return (IEnumerator)this;
	}
	
	//IEnumerator  //move the enumerator to the next element
	public bool MoveNext(){
			position ++;
			return (position < zombieArray.Length);
	}
	
	//IEnumerator  //reset the enumerator to the first element
	public void Reset(){
		position =0;
	}
	
	//IEnumerator  //Property that provides access to the current enumerated object
	public object Current{
		get{  return zombieArray[position];  
		}
	}
	
	
}