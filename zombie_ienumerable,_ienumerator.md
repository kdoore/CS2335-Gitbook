#Zombies: IEnumerable, IEnumerator

The IEnumerable and IEnumerator interfaaces 

We want to create our own collection class:  Zombies.  This class will allow us to store a number of zombie objects.  We can use a variety of different data structures such as an array or a list within our collection, to store our individual zombie instances, but we want our implementation choice of data structures to be hidden to enforce encapsulation for our class. In order to provide access to elements of our Zombies collection, we want to implement the C# IEnumerable and the IEnumerator interfaces which allow us to use foreach to operate on each element of our collection.

If our Zombies collection can be accessable by users of our class using the foreach method, then we can say that our collection is *Enumerable*, this means that we have implemented an *Enumerator*.  Since we are going to implement these interfaces, then we need to look at the interface specification so we understand what code we are required to implement to fulfill the interface contract.

