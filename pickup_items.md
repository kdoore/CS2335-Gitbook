# PickUp Items
Throughout our game we will want to have game items for the player to interact with.  Often these are considered Pick-up items.  So, we'll want to create a base-class that represents this Pick-up type and create child classes to extend for specialized pick-up items.  Some of our pick-up objects will need the ability to generate events that can notify other objects when the pick-up item has died.  Therefore, we'll create a Delegate: OnDiedHandler which specifies the signature of any eventHandler that wants to register for notification when the died event occurs and the OnDied notifications are invoked.

using UnityEngine;
using System.Collections;
using System;

public enum PickupType { crystal, star, animatedCrystal, purpleCrystal };

public class PickUp : MonoBehaviour {
	/// <summary>
	/// On died handler. Declare in the base class 
	/// and use in all child classes
	/// </summary>
	public delegate void onDiedHandler( PickUp thisPickup);

	/// <summary>
	/// Occurs when on died. Use in base class and child classes
	/// </summary>
	public event onDiedHandler onDied; 

	public PickupType type;
	public int value;

	public void DestroyMe(){
		Debug.Log ("Item Destroy Me");
		Destroy(gameObject);
	}
	////since we want to use within animation, Unity won't let it be 
	/// declared as a virtual method, this is a Unity bug. 
	/// <summary>
	/// Died this instance.
	/// And Invoke onDied event: execute each eventHandler 
	/// </summary>
	public void Died(){
		if (onDied != null) {
			onDied (this);  // invoke event, and execute each 
							// referenced eventHandler
		}
		DestroyMe ();  // then destroy the gameObject
	}


}


