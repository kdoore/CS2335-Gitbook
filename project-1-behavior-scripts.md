#Project 1 - Custom Scripts for Script Components

To give our gameObjects the desired behavior, we'll write several custom C# scripts, one for each type of GameObject, and one to control the overall game.  

###Create Script Assets
In the Asset pane of the Project panel, right click and Create > C# Script.  Name the script: Apple.  Repeat to create the following scripts:  AppleTree, Basket, GameController.  All scripts must be capitalized to meet Visual Studio's requirement that all class names must be capitalized.   Double-click on one of the scripts to open Visual Studio.  I recommend making sure that you open Visual Studio by clicking on a script-file in your open project to insure that the script files you modify are actually associated with the project you are working on.  

###Apple Class
The following class code determines when the apple's have fallen below the bottom of the screen, then they are destroyed.  


```java

using System.Collections;
using System.Collections.Generic;
using UnityEngine;

public class Apple : MonoBehaviour {


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

###Basket Class
For the Basket, which catches falling objects, we'll make some simplifications compared to the textbook.  First, there's no need to create 3 instances of this object since the mouse is used to control the movement, 3 objects would all simply behave as a single object.  Second, there's no need to dynamically create the object, we can just place it directly in the scene from the beginning.  The Basket class code allows the mouse to move the basket, and checks for collisions with the falling Apple objects.  The code to do this is somewhat cryptic, as it requires the mouse's screen positioning to be translated into world coordinates.  After we determine the mouse's position in the world, we simply set the basket's transform.position using this calculated mouse position.


  
