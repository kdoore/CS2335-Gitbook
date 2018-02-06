#Project 1 - Custom Scripts for Script Components

To give our gameObjects the desired behavior, we'll write several custom C# scripts, one for each type of GameObject, and one to control the overall game. We'll make further changes to some of these scripts, but the scripts below are the starting script versions for the game. 

###Create Script Assets
In the Asset pane of the Project panel, right click and Create > C# Script.  Name the script: Apple.  Repeat to create the following scripts:  AppleTree, Basket, GameController.  All scripts must be capitalized to meet Visual Studio's requirement that all class names must be capitalized.   Double-click on one of the scripts to open Visual Studio.  I recommend making sure that you open Visual Studio by clicking on a script-file in your open project to insure that the script files you modify are actually associated with the project you are working on.  

###Apple Class - Apple Script Component
The following class code determines when the apple's have fallen below the bottom of the screen, then they are destroyed.
Attach this script to the Apple Prefab-GameObject to create a script component.  


```java

using System.Collections;
using System.Collections.Generic;
using UnityEngine;

public class Apple : MonoBehaviour {


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
###Rock.cs or PickUp.cs
Similarly, we should create a script like the Apple script for any other dropped objects.  Or, we can create a script called PickUp, that can work for all dropped objects.

 

```java

public class PickUp : MonoBehaviour
{

    //this variable tracks the lowest point the apples should reach before 
    //being destroyed, this is the same for all appples, so it's been
    //set as a class-level, static, variable and will not show up in the inspector.
    public static float bottomY = -12f;

    public int pointValue;

    // Update is called once per frame
    void Update()
    {
        //check the Y position value of transform of the gameObject this 
        //script is attached to, if it's below the value of bottomY, then
        //destroy the gameObject this script component is attached to.
        if (transform.position.y < bottomY)
        {
            Destroy(this.gameObject);
        }
    }
} 
```





###Basket Class
For the Basket, which catches falling objects, we'll make some simplifications compared to the textbook.  First, there's no need to create 3 instances of this object since the mouse is used to control the movement, 3 objects would all simply behave as a single object.  Second, there's no need to dynamically create the object, we can just place it directly in the scene from the beginning.  The Basket class code allows the mouse to move the basket, and checks for collisions with the falling Apple objects.  The code to do this is somewhat cryptic, as it requires the mouse's screen positioning to be translated into world coordinates.  After we determine the mouse's position in the world, we simply set the basket's transform.position using this calculated mouse position. 

The following code is in the Basket Class, it causes the Basket's X position to be recalculated so matches the mouses X position.   

Attach the full Basket class script to the Basket GameObject-Prefab to create a script component
	

```java
// In the Basket Class
	void Update () {
	//
        Vector3 mousePos2D = Input.mousePosition; //temp variable to store the mousePosition on the screen
        mousePos2D.z = -Camera.main.transform.position.z; //z position caclulation based on camera z position
        Vector3 mousePos3D = Camera.main.ScreenToWorldPoint(mousePos2D); //calculate the mouse position inside the world space coordinate system

        Vector3 pos = this.transform.position; //temp variable that is the Basket's current position
        pos.x = mousePos3D.x; //update Basket's X position so it's equal to mouseX
        this.transform.position = pos; //update basket's position
	}
```

###Basket-Apple Collisions and Apple Custom Tags
To test for collision with falling Apples, we'll use the OnCollisionEnter2D( ) Unity event function.  Once a collision event has occurred, the code we write in this eventHandler function is executed, but we need to make sure that the object that we've collided with is actually an apple. So, follow the [Unity Manual to create a new Tag:](https://docs.unity3d.com/Manual/Tags.html) Apple, and then add that tag to the Apple prefab, and hit Apply to make the Tag 

The code below is in the Basket Class, it determines when the basket collides with an apple, then destroys the Apple GameObject.

```java

//In the Basket Class
    void OnCollisionEnter2D(Collision2D collision)
    {
        GameObject collidedWith = collision.gameObject;
        if(collidedWith.tag == "Apple"){
            Debug.Log("Collision");
            Destroy(collidedWith); //destroy Apple GameObject
        }
    }

```

 ###Apple Tree Class 
The code below is for the AppleTree class.  This code must be attached to the AppleTree GameObject-Prefab.

```java
using System.Collections;
using System.Collections.Generic;
using UnityEngine;

public class AppleTree : MonoBehaviour {

    [Header("Set in Inspector")]
    //prefab for apples that will be spawned
    public GameObject applePrefab;
    //public GameObject rockPrefab; // we'll use this later


    //speed the tree moves horizonatlly
    public float speed = 1f;

    //left right edges of the canvas
    public float leftRightEdge = 10f;

    public float chanceToChangeDirections = 0.01f;  //set to small probability
    
    //public float chanceToDropRock = 0.01f //we'll add and use this later

    public float secondsBetweenAppleDrops = 1f;

	// Use this for initialization
	void Start () {
        Invoke("DropObjects", 2f);  //this code will be removed later, and instead we'll call this function when the StartButton has been clicked
	}
	
	// Update is called once per frame
	void Update () {
        Vector3 pos = transform.position;
        pos.x += speed * Time.deltaTime;
        transform.position = pos;

        if( pos.x < -leftRightEdge){
            speed = Mathf.Abs(speed); //make sure speed is positive
        }else if( pos.x > leftRightEdge){
            speed = -Mathf.Abs(speed); //make sure speed is negative
        }
	}

    void FixedUpdate()
    {
        if(Random.value < chanceToChangeDirections){
            speed *= -1;
        }
    }

//this function causes apples to be dropped from the apple tree
//we'll modify in later, we'll add code to also drop rocks
    public void DropObjects(){
        GameObject apple = Instantiate<GameObject>(applePrefab);
        apple.transform.position = this.transform.position;
        Invoke("DropObjects", secondsBetweenAppleDrops);
    }
}

 
```


