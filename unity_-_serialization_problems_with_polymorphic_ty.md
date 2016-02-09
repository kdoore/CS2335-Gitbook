# Unity - Serialization Problems with Polymorphic Types



Polymorphism is one of the three main pillars of object-oriented programming, unfortunately, Unity does not provide great support for using polymorphic data-types in conjunction with Serialization.  There are several good articles and blogs  that discuss the details of the issues:

- [Serialization of Polymorphic Data-Types](https://feedback.unity3d.com/suggestions/serialization-of-polymorphic-dat)
- [Advanced Unity Serialization](http://www.codingjargames.com/blog/2012/11/30/advanced-unity-serialization/)
- [Unity Serialization](http://blogs.unity3d.com/2014/06/24/serialization-in-unity/) 

The code snippet below gives an example of the situation:
```
[Serializable] class Base { public int myInt; }
[Serializable] class Derived : Base { public string myString; }
 
class MyBehaviour : MonoBehaviour
{
   public Base  _myBase = new Derived(); // Error... This will NOT work, only myInt will be saved.
}
```
