#Project 1 - Updates to Custom Scripts

The following changes need to be made to the custom scripts created earlier: AppleTree and Basket.  In addition, a Rock script is created to add a destructive object to the game. The Rock script is similar to the Apple.cs script, but the pointValue is negative.  

###GameController.gameActive to Control Gameplay Actions 

Since we don't want apples being dropped before the StartGame button has been selected, and because we want to stop apples from dropping when the game has ended, we can create a boolean variable in the GameController class: `bool gameActive`.  We can toggle the value of this variable to start / stop apples from dropping; since apple dropping is managed in the AppleTree script, we need to give this variable a `public` access modifier.  

###AppleTree - No Tree Movement unless gameActive is true

We need to modify the AppleTree.cs class to make sure the AppleTree gameObject doesn't move until the StartButton is clicked.  We do this by checking whether GameController.gameActive is true or false.  First we need to create a script Object-Reference variable to allow us to interact with the GameController script component within the AppleTree.cs script.  Since we want to access a variable outside the class definition, we need to declare the variable to have public access.

//Inside GameController.cs:

```java
public bool gameActive = false; //initialize to false
```

###AppleTree.cs Script Changes
Inside AppleTree.cs, first we must declare an object-reference variable that will allow us to interact with the GameController script-component object attached to the GameController GameObject

In Start( ), we initialize our object-reference, so that it's memory address points to the GameController script component-object, to do that, first we must find the GameController GameObject, then we use GetComponent<T>(); function to find the GameController script component.

In Update, we test the value of the: gameContoller.gameActive variable, if gameActive is true, then the AppleTree can have horizontal Movement.  If gameActive gets reset to false, the appleTree movement stops.


```java
//To access inside AppleTree.cs
//declare an Object Reference - the dataType is the GameController Class Name
private GameController gameController;  
 
void Start () {  
        //find the GameController GameObject by name
        // then find the ScriptComponent using GetComponent<T>( ); function
        gameController = GameObject.Find("GameController").GetComponent<GameController>();

// Update is called once per frame
void Update () {
        if (gameController.gameActive)  //add this code
        {
            Vector3 pos = transform.position;
            pos.x += speed * Time.deltaTime;
            transform.position = pos;

            if (pos.x < -leftRightEdge)
            {
                speed = Mathf.Abs(speed); //make sure speed is positive
            }
            else if (pos.x > leftRightEdge)
            {
                speed = -Mathf.Abs(speed); //make sure speed is negative
            }
        }
	}
////OTHER CLASS CODE NOT SHOWN HERE
```

 ###Control Dropping Objects in AppleTree.cs 

```java
public void DropObjects()
    {
        if (Random.value < chanceToDropRock)
        {
            GameObject rock = Instantiate<GameObject>(rockPrefab);
            rock.transform.position = this.transform.position;
        }
        else { 
            GameObject apple = Instantiate<GameObject>(applePrefab);
            apple.transform.position = this.transform.position;
        }
        //Add this code so we only call Invoke if  gameController.gameActive is true.
        if (gameController.gameActive) { 
        Invoke("DropObjects", secondsBetweenAppleDrops);
        }
    }

```

###Basket.cs Code Changes

Just as we did in the AppleTree.cs class, we need to create an object-reference variable to allow us to interact with the GameController script component.  Here is the partial code from the Basket.cs class, showing the changed code:

###OnCollisionEnter2D( ) - Changes
In the Basket class, we have modified the code in the OnCollisionEnter2D( ) function.  We want to update the score in the GameController each time we collide with an object, in order to do that, we need to find out how many points each object is worth.  

Below we create an object reference variable to access the Apple script component, if we know we collided with an apple.


```java
Apple apple = collidedWith.GetComponent<Apple>(); 
```

Then we call the UpdateScore function of the gameController script component and pass along the points associated with the apple object.


```java
gameController.UpdateScore(apple.pointValue);
```


###Modified Code in Basket.cs 
   
```java
private GameController gameController;

void Start(){
        gameController = GameObject.Find("GameController").GetComponent<GameController>();
    }
	// Update is called once per frame
void Update () {
        if (gameController.gameActive)
        {
            Vector3 mousePos2D = Input.mousePosition;
            mousePos2D.z = -Camera.main.transform.position.z;
            Vector3 mousePos3D = Camera.main.ScreenToWorldPoint(mousePos2D);

            Vector3 pos = this.transform.position;
            pos.x = mousePos3D.x;
            this.transform.position = pos;
        }
	}


void OnCollisionEnter2D(Collision2D collision)
    {
        GameObject collidedWith = collision.gameObject;
        if(collidedWith.tag == "Apple"){
            Debug.Log("Collision with Apple");
            Apple apple = collidedWith.GetComponent<Apple>();
            gameController.UpdateScore(apple.pointValue);
            Destroy(collidedWith);
        }else if(collidedWith.tag =="Rock"){
            Rock rock = collidedWith.GetComponent<Rock>();
            gameController.UpdateScore(rock.pointValue);
            Destroy(collidedWith);
        }
    }	 
```

