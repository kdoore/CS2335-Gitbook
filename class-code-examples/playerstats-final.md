#PlayerStats - Final

Updated 4/24/2019

Includes code for LevelManager to determine max level score.

```java
using UnityEngine;
using UnityEngine.UI;

public class PlayerStats : MonoBehaviour {

    public Text scoreText; //populate in the inspector
    public Text healthText;

 
	// Use this for initialization
	void Start () {
        ////make connection with Text component on GameObject
           UpdateDisplay();
        GameData.instanceRef.onPlayerDataUpdate.AddListener(UpdateDisplay);
    }
	

    void UpdateDisplay()
    {
        healthText.text = "Health: " + GameData.instanceRef.Health;
        scoreText.text = "Total Score: " + GameData.instanceRef.Score  ;
    }

}//end class
```

