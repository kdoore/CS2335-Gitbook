#Project 1 - Custom Scripts for Script Components

To give our gameObjects the desired behavior, we'll write several custom C# scripts, one for each type of GameObject, and one to control the overall game.  

###Create Script Assets
In the Asset pane of the Project panel, right click and Create > C# Script.  Name the script: Apple.  Repeat to create the following scripts:  AppleTree, Basket, GameController.  All scripts must be capitalized to meet Visual Studio's requirement that all class names must be capitalized.   Double-click on one of the scripts to open Visual Studio.  I recommend making sure that you open Visual Studio by clicking on a script-file in your open project to insure that the script files you modify are actually associated with the project you are working on.  

###Apple Class

using System.Collections;
using System.Collections.Generic;
using UnityEngine;

public class Apple : MonoBehaviour {

The following class code determines when the apple's have fallen below the bottom of the screen, then they are destroyed.  

```java
    //this variable tracks the lowest point the apples should reach before 
    //being destroyed, this is the same for all appples, so it's been
    //set as a class-level, static, variable and will not show up in the inspector.
    public static float bottomY = -12f;

	
	// Update is called once per frame
	void Update () {
        //check the Y position value of transform of the gameObject this 
        //script is attached to, if it's below the value of bottomY, then
        //destroy the gameObject this script component is attached to.
        if(transform.position.y < bottomY){
            Destroy(this.gameObject);
        }
	}
}

```

