#Interfaces

Interfaces in C# are similar to base-class inheritance because they provide structured code that is implemented in another class. Interfaces can include both Properties and Methods. For interface methods no implementation code is defined in the interface, only the method signature is specified. Interfaces define a contract that must be implemented by any class that implements the interface.  

One important feature of interfaces is that a class may implement any number of interfaces, whereas a class can only inherit from 1 base-class.  Interfaces provide template to specify consistent functionality across several different classes.

###IDamage Interface

Below is the code that we designed in class to create the IDamage interface and implement it in our Zombie class.

```
using UnityEngine;
using System.Collections;

//define IDamage interface 
interface IDamage  {
	 //method declaration these methods must be defined in any class that implements the IDamage interface
	 
	 void TakeDamage(int damage);
	 void HealDamage(int damage);
}
```

###Zombie Class implements IDamage

```
using UnityEngine;
using System.Collections;

public class Zombie :  IDamage {
	//class level property
	public static int NumberOfZombies = 0;

	// private fields - hold object state
	private string name;
	private float friends;  //can change the implementation details without breaking the property
	private int brainsEaten;
	private int zombieHitPoints;
	private GameObject ZombieMesh;
	
	#region properties
	public string Name{  
		get{
			return name;
		}
		set{
			name = value;
		}
	}
	public int Friends{  
		get{
			return (int)friends;  //do a typecast to convert internal float to return an int value
		}
	
		set{ 
			if(value >=0){
			friends = (float)value;  //we can do a typecast to deal with a field data change
			}
		} 
	}
	int BrainsEaten { 
		get{ 
			return brainsEaten;
			}
		set{ 
			brainsEaten= value;
			}
	 }
	 
	
	public int HitPoints{   //implemment IDamage Interface
		get{ 
			return zombieHitPoints;
		}
		set{ 
			if(value >=0){
			zombieHitPoints = value;
			}
		}
	} 
	#endregion 
	// constructor
	public Zombie( string n, int hp){
		NumberOfZombies=1;
		Name = n;
		HitPoints= hp;
		Friends=0;
		ZombieMesh = GameObject.CreatePrimitive(PrimitiveType.Capsule);
		Vector3 pos = new Vector3(Random.Range (-10,10),0,Random.Range (-10,10));
		ZombieMesh.transform.position= pos;
		
	}
	
	
	//methods
	public override string ToString ()
	{
		return "Zombie: " + Name + " brains eaten: " + brainsEaten ;
	
	}
	
	//IDamage methods
	public void TakeDamage( int damage){
		zombieHitPoints -= damage;
	}
	
	public void HealDamage( int damage){
		return;        //zombies can't be healed, no code impleented
	}

}
```