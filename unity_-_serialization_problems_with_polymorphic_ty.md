# Unity - Serialization Problems with Polymorphic Types

Polymorphism is one of the three main pillars of object-oriented programming, unfortunately, Unity does not provide seamless support for using polymorphic data-types in conjunction with Serialization.  There are several good articles and blogs  that discuss the details of the issues.  There are options to deal with this issue which requires creation of custom serialization scripts for these objects.  Some custom scripts are mentioned in the articles below. 

- [Unity Serialization](http://blogs.unity3d.com/2014/06/24/serialization-in-unity/) 
- [Serialization of Polymorphic Data-Types](https://feedback.unity3d.com/suggestions/serialization-of-polymorphic-dat)
- [Advanced Unity Serialization](http://www.codingjargames.com/blog/2012/11/30/advanced-unity-serialization/)


The code snippet below gives an example of the situation. When serialized, the base-class type is actually serialized, not the derived-type, if using a base-class reference.

```
[Serializable] class Base { public int myInt; }
[Serializable] class Derived : Base { public string myString; }
 
class MyBehaviour : MonoBehaviour
{
   public Base  _myBase = new Derived(); // Error... This will NOT work, only myInt will be saved.
}
```
