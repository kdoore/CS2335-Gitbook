Zombie Class

In chapter 6, section 4, the author discusses Class Constructors. He creates a Zombie class.  In the code below, we have expanded his example to show how to use Properties in C# to provide access to private instance fields.  

In this code example, we want to explore C# Encapsulation, Properties, Access Modifiers, Static Fields. In addition we want to introduce the C# Array syntax as well as the syntax to override methods. We'll also look at how to instantiate game objects in a C# script.

###Zombie Custom Class
First we'll create a custom class that does not inherit from MonoBehaviour and see how we can use these types of classes in our projects.  

Zombie.cs
```
using UnityEngine;
using System.Collections;

public class Zombie  {
	
	//class level property
	public static int NumberOfZombies = 0;

	//instance variables
	public int brainsEaten;
	public int hitPoints;
	private GameObject ZombieMesh; 
	
	//fields
	private string name;
	
	public string Name{  //property
		get{
			return name;
		}
		set{
			name = value;
		}
	}
	//property
	public int Friends{  get; set; }
	
	// constructor
	public Zombie( string n, int hp){
		NumberOfZombies++;
		name = n;
		hitPoints= hp;
		Friends=0;
		ZombieMesh = GameObject.CreatePrimitive(PrimitiveType.Capsule);
		Vector3 pos = new Vector3(Random.Range (-10,10),0,Random.Range (-10,10));
		ZombieMesh.transform.position= pos;
		
	}
	
	//
	public override string ToString ()
	{
		return "Zombie: " + Name + " brains eaten: " + brainsEaten;
	}
```
	
}

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
	
	// Update is called once per frame
	void Update () {
	
	}
}
```
