Zombie Class

In chapter 6, section 4, the author discusses Class Constructors. He creates a Zombie class.  In the code below, we have expanded his example to show how to use Properties in C# to provide access to private instance fields.  

using UnityEngine;
using System.Collections;

public class Zombie  {
	//class level property
	public static int NumberOfZombies = 0;

	//instance variables
	public int brainsEaten;
	public int hitPoints;
	GameObject ZombieMesh;
	
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

	
}
