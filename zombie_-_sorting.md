#Zombie - Sorting

What does it mean to *sort* a list of zombies?  The process of sorting means that there is a defined order of value for each element of the collection and that we can compare any 2 elements of the collection and determine which element has the higher value, are they equal valued, and which has the lower value.  So for every comparison of 2 elements of the collection, there are 3 possible outcomes.  We select one element, then compare to an *other* element, our primary element is either: Greater than, Equal to, or Less than the *other* element.  

In the code below, we implement the generic interface:  IComparable < T >, where we're specifying that the Type of object comparison we want to implement is for *Type: Zombie*.  Below implement this interface for our Zombie class, this will allow comparison of pairs of Zombie object instances. IComparable < Zombie >

###IComparable < T >


