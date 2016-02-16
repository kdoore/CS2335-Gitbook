# IStateBase
IStateBase is a custom interface that we will have all StateX classes implement.  This guarantees that all States will implement all Properties and Methods defined in this interface.

In StateManager, we will use an IStateBase object reference to keep track of the current active state instance.  

```
// in StateManager

IStateBase activeState;  //

void Start(){
    activeState = new BeginState( this );  //reference to first activeState
}

```

```
using UnityEngine;
using System.Collections;

/// <summary>
/// I state base.
/// Interface for all StateX.cs classes
/// </summary>
public interface IStateBase{
		
	gameState State // Inteface Property
	{
		get;
	}    

	//all interface methods are public by default!

	void StateUpdate (); //executed by StateManager in Update(), executed once per frame

	void StateGUI (); // can execute any onGUI() code

	void InitializeObjectRefs ();  //cache object References once Scene is loaded
}


```

