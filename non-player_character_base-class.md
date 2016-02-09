# Non-Player Character: Base-Class

            
###Zombie's Base-Class: NPCharacter
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
	
	public virtual void doSomething(){  
		Debug.Log("NPC do something");
	}
		
	public override string ToString ()
	{
		return string.Format ("NPC Name: {0}, HitPoints {1}", name, hitPoints);
	}
}

```

###Protected Access Modifier:
Notice that we have changed the access modifier for each of the class-instance/field variables.  When these were part of the Zombie class, we had them as private to enforce encapsulation and restrict access and modification of these variables.  However, using the private access modifier also restricts access to these variables even for child-classes.  It might make sense that we need access to these variables within a child class so the `protected` access modifier is designed specifically for this purpose. 

###UML Diagram

The diagram below shows the Unified Modeling Language model for the classes for this stage of our project.  UML class diagrams provide a visual overview of the relationships between classes, interfaces, and the features: variables and methods for each of these classes.  

![](NPCharacterUML.png)

###Serialization Problems in Unity when Using Polymorphic Types

Polymoprhism is one of the three main pillars of object-oriented programming, unfortunately, Unity does not provide great support for using polymorphic types in conjunction with Serialization.  There are several good articles and blogs  that discuss the details of the issues:

- [SERIALIZATION OF POLYMORPHIC DATA TYPES](https://feedback.unity3d.com/suggestions/serialization-of-polymorphic-dat)
- [Advanced Unity Serialization](http://www.codingjargames.com/blog/2012/11/30/advanced-unity-serialization/)

The code snippet below gives an example of the situation:
```
[Serializable] class Base { public int myInt; }
[Serializable] class Derived : Base { public string myString; }
 
class MyBehaviour : MonoBehaviour
{
   public Base  _myBase = new Derived(); // Error... This will NOT work, only myInt will be saved.
}
```
