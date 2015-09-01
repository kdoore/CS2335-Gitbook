# Unity Game Engine

In this course we'll learn to create programs in the Unity Game Engine

- We will use the opensource MonoDevelop IDE (integrated development environment). Another option is to use Microsoft Visual Studio for your development environment.


Below is an example quiz:
<quiz name="Quiz1">
    <question multiple>
        <p>What is MonoDevelop?</p>
        <answer correct>An application to edit programs for Unity</answer>
        <answer>A game engine integrated with Unity</answer>
        <answer correct>An application to debug Unity programs</answer>
        <explanation>MonoDevelop is an integrated development environment that supports editing and debugging programs for Unity.</explanation>
    </question>
    <question>
        <p>Is this a quiz?</p>
        <answer correct>Yes</answer>
        <answer>No</answer>
    </question>
</quiz>

{%ace edit=false, lang='c_cpp'%}
// This is a script to move a game object along the x axis

using UnityEngine;
using System.Collections;

public class controller : MonoBehaviour {

	public int myValue;
	public Vector3 myVector;
    
	// Use this for initialization
	void Start () {
		myValue=5;
		myVector = new Vector3(1,1,0);
		transform.position=myVector;
		Debug.Log ("MyValue= " + myValue);
	}
	
	// Update is called once per frame
	void Update () {
	     myVector.x += .1f;
	     transform.position=myVector;
	     Debug.Log ("xPosition= " + myVector.x);
	     
	}
}
{%endace%}



