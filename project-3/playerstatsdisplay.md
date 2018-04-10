#PlayerStatsDisplay
The following code can be used to update UI Game Items to display the totalScore and health.  The UI gameObjects must have the same name as listed in the code below.


```java

using System.Collections;
using System.Collections.Generic;
using UnityEngine;
using UnityEngine.UI;

public class PlayerStatsDisplay : MonoBehaviour {

    private Text scoreText, healthText;


	// Use this for initialization
	void Start () {
        scoreText  = GameObject.Find("ScoreText").GetComponent<Text>();
        healthText = GameObject.Find("HealthText").GetComponent<Text>();
        GameData.instanceRef.onPlayerDataUpdate.AddListener(UpdateDisplay);
        UpdateDisplay();
    }

    void UpdateDisplay(){
        Debug.Log("Score Updated");
        scoreText.text = "Score: " + GameData.instanceRef.TotalScore;
        healthText.text = "Health: " + GameData.instanceRef.Health;
    }
}
```

