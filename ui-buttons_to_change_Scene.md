#UI Buttons To Change Scene

In Unity, we can use Scenes to implement game levels.  Using the StateManager framework, each StateX.cs script has scene change, event-logic, where we'll write custom methods to be executed when buttons are clicked in the scene that correspond to scene change events.

###2 Scene-Change Events in each Scene / State.   


1. Add 2-Buttons to each Scene
2. Add all scenes to the edit => Build Settings  => Scenes
3. Add code in StateX.cs file to:
 A.  Find the Button Component
 B.  Write a custom method to change scenes/ states
 C.  Add the custom method to the OnClick event of the Button component in script
 D.  Run the scene and see if the Button Works

###Add Scenes to Project Build Settings


###Control UI Elements via Code
Although there are methods that allow us to use the inspector to determine which method gets executed when a Button is clicked. Since Unity can  is preferable to implement this logic within a custom script.  this will make it easier to extend our project and to debug issues.  The code below shows how we can create an object-reference variable which will give us access to a the <Button> Component of a Button GameObject. It is straightforward to write code to have a custom method executed when the button is clicked.  Here we have created a method:  LoadEndState(), this will switch to the Scene named: "EndScene".  

We must create a custom method that we want to have executed by the Button component's onClick event, we'll learn more about passing functions to the AddListener Method, but essentially, we are using the method name: LoadBeginState, as an argument to AddListener( ), this registers our LoadBeginState as a Listener to this button's onClick event.

###Button Logic in BeginState to Switch Scenes
```java

using UnityEngine;
using UnityEngine.UI;
using System.Collections;
using UnityEngine.SceneManagement;

public class BeginState : IStateBase
{
       //other class code is missing here

	private Button endBtn
	
	public void InitializeObjectRefs ()
	{
		endBtn = GameObject.Find ("EndButton").GetComponent<Button> ();
		endBtn.onClick.AddListener (LoadEndScene);
 
		Debug.Log ("Add Debug Info");
	}
 
 
	/// <summary>
	/// Event handler - called when endBtn is clicked
	/// Loads the end scene.
	/// </summary>
	public void LoadEndScene ()
	{  
		Debug.Log ("Add Debug Info");
		SceneManager.LoadScene ("EndScene"); //Unity event function to Load new scene by name
		StateManager.managerInstance.SwitchState (new EndState ()); //StateManager code to change State
	}
}//end class

```