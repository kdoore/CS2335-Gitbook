###Apple Tree Class
The code below is for the AppleTree class. This code must be attached to a GameObject.  If there's no Spite associated with this gameObject, we can actually consider it a simple spawner.

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

public float chanceToChangeDirections = 0.01f; //set to small probability
//public float chanceToDropRock = 0.01f //we'll add and use this later

public float secondsBetweenAppleDrops = 1f;

// Use this for initialization
void Start () {
Invoke("DropObjects", 2f); //this code will be removed later, and instead we'll call this function when the StartButton has been clicked
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