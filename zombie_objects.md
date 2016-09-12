#Zombie Custom Objects

Code Example from: 

* Learning C# Programming with Unity 3D
* By: Alex Okita
* Chapter 6, Section 4.

(Book available through McDermott Library as an eBook);
 

In this code example, we want to explore C# Encapsulation, Properties, Access Modifiers, Static Fields. In addition we want to introduce the C# Array syntax as well as the syntax to override methods. We'll also look at how to instantiate game objects in a C# script.

###Zombie Custom Class
First we'll create a custom class that does not inherit from MonoBehaviour and see how we can use these types of classes in our projects.   Since the class does not inherit from MonoBehaviour, we removed the ``Start()``, ``Update()`` code and the ``:MonoBehaviour`` base class specification. If you compile this code and try to add it to any GameObject in your scene, you will get an error.  The only custom scripts that can be added as a script component to a GameObject are classes that inherit from MonoBehaviour.  

Below we create the custom Zombie Class that we'll use to dynamically spawn a gameObject within our scene.  In order to have our Zombie class used in a scene, we'll need to create an instance of a Zombie within a custom class that inherits from MonoBehaviour. We'll create the Example.cs class for that purpose, and we'll attach it to the main camera within our game scene.

###Properties
C# Language provides Properties as a feature to support encapsulation, Properties 

Zombie.cs
```Csharp
using UnityEngine;
using System.Collections;

public class Zombie  {
	
	//class level property
	public static int NumberOfZombies = 0;

	//instance variables
	public int brainsEaten;  //should be private with an associated Property
	
	private int hitPoints;
	private string name;
	private GameObject ZombieMesh; 
	private GameObject ZombiePrefab;

	#region Properties
    public float HitPoints{       //myZombie.HitPoints = 4;
		set{
			hitPoints = (int)value;

			  ///event has happened to change this value
			/// call function to update something somewhere else 
		}
		get{
			return hitPoints;
		}
	}
	
	public string Name{  //property
		get{
			return name;
		}
		set{
			name = value;
		}
	}
	//property auto-initialize - not recommended for this course
	public int Friends{  get; set; }
	
	#endregion
	
	// constructor
	public Zombie( string n, int hp){
		NumberOfZombies++;
		name = n;
		hitPoints= hp;
		Friends=0;
		
		///the code below will create a visible object in a scene that is associated with our custom class
		
		ZombieMesh = GameObject.CreatePrimitive(PrimitiveType.Capsule);
		Vector3 pos = new Vector3(Random.Range (-10,10),0,Random.Range (-10,10));
		ZombieMesh.transform.position= pos;
		
		//instantiate a prefab via code: 
		//the prefab: named "BlueZombie" must be in the Resources folder within the assets folder
		
		ZombiePrefab = GameObject.Instantiate (Resources.Load ("BlueZombie")) as GameObject;

		// here we initialize directly while calling the Vector3 constructor directly
		ZombiePrefab.transform.position = new Vector3 (Random.Range (-5, 5), 0, Random.Range (-5, 5));;

	}
	
	//
	public override string ToString ()
	{
		return "Zombie: " + Name + " brains eaten: " + brainsEaten;
	}
}
```

###Example.cs
In the Example.cs file, we declare an array named zombies of Zombie objects, then in the Start method we initialize the Zombie objects and we output their Names in the console using Debug.Log().
```
using UnityEngine;
using System.Collections;

public class Example : MonoBehaviour {

	private Zombie[] zombies = new Zombie[3];   //create an array

	// Use this for initialization
	void Start () {
			string[] names= new string[]{"Stubbs", "Rob", "White"};
			for(int i=0; i< names.Length; i++){
				zombies[i]= new Zombie(names[i], Random.Range (10,15));
				Debug.Log (zombies[i].ToString ());
			}
		Debug.Log ("first guy " + zombies[0].Name);
		zombies[0].Name= "Charles";
	}
	
}
```
