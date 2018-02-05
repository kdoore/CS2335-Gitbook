###GameController gameActive To Control Behavior of AppleTree and Basket.

The final step of the project is to make sure we don't have the AppleTree or Basket moving before the game has started, and we also don't want any objects dropping if the game hasn't started.  We have previously defined a bool variable:  gameActive in GameController.cs.  Now we need to add code to the Basket and AppleTree classes so we can using that variable to control movement of these objects.

###Basket.cs  
For the Basket.cs code, we need to declare an object reference variable so we can access the GameController.cs variable: gameActive.

**Declare** An object reference variable for the GameController script component


```java
 //Basket.cs
 private GameController gameController;
```
**Initialize** in Start( ) - find the GameObject: GameController, get the GameController script component on that gameObject.



```java
 void Start(){
        gameController = GameObject.Find("GameController").GetComponent<GameController>();
    }
```



**Access and Use: gameActive**
We'll use the gameController.gameActive variable to wrap all code inside the Basket.cs Update function - so no code will be executed here unless gameController.gameActive is true.  This will keep the basket from moving until the game has been started.	

```java
void Update () {
        if (gameController.gameActive) ///ADD THIS CODE
        { //ADD This open curly-brace
        
        ///NO NEW CODE BELOW HERE
            //find out the mouse's position
            Vector3 mousePos2D = Input.mousePosition;
            //mousePos2D.z = -Camera.main.transform.position.z;
            mousePos2D.z = 0f;
            Vector3 mousePos3D = Camera.main.ScreenToWorldPoint(mousePos2D);

            Vector3 pos = this.transform.position;
            pos.x = mousePos3D.x;
            //the code below keeps the basket on the screen by restricting the pos.x value
            pos.x = Mathf.Clamp(pos.x, -leftRightEdge, leftRightEdge);
            this.transform.position = pos;
        //NO NEW CODE ABOVE HERE
    
    
        } //ADD THIS - close curly-brace
	} //End of Update
```

