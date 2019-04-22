#PlayerStats - Final

Includes code for LevelManager to determine max level score.

```java
using UnityEngine;
using UnityEngine.UI;

public class PlayerStats : MonoBehaviour {

    public Text scoreText; //populate in the inspector
    public Text healthText;
    public LevelManager levelManager;
 
	// Use this for initialization
	void Start () {
        ////make connection with Text component on GameObject
           UpdateDisplay();
        
    }
	
	// Update is called once per frame
	void Update () {  //Polling - rather use custom events 
        UpdateDisplay(); //this is inefficient
	}


    void UpdateDisplay()
    {
        healthText.text = "Health: " + GameData.instanceRef.Health;
        scoreText.text = "Score: " + GameData.instanceRef.Score + " of  "  + levelManager.maxLevelScore ;
    }

}//end class

```

