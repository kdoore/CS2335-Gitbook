#PlayerStatsDisplay
The following code can be used to update UI Game Items to display the totalScore and health.  The UI gameObjects must have the same name as listed in the code below. (See below for version with public variables which must be populated in the inspector)
    - StatsDisplay is a UIPanel
    - ScoreText is UIText (child of the UIPanel)  
    - HealthText is UIText (child of the UIPanel)  
    - Set the RectTransforms for the ScoreText so it's stretched and anchored to the top of it's parent, UIPanel
    - Set the RectTransform for the HealthText so it is stretched and anchored to the bottom of it's parent, UIPanel 

![](/assets/Screen Shot 2018-04-12 at 11.05.02 AM.png)

![](/assets/Screen Shot 2018-04-12 at 11.08.46 AM.png)

```java
//code updated 4/12/18 11:00am
using System.Collections;
using System.Collections.Generic;
using UnityEngine;
using UnityEngine.UI;
using UnityEngine.Events;

public class PlayerStatsDisplay : MonoBehaviour {

    private Text scoreText, healthText;

	// Use this for initialization
	void Start () {
        scoreText  = GameObject.Find("ScoreText").GetComponent<Text>();
        healthText = GameObject.Find("HealthText").GetComponent<Text>();
        GameData.instanceRef.onPlayerDataUpdate.AddListener(UpdateDisplay);
        UpdateDisplay(); //call once to set initial values
    }

    //Executed each time score / heath is updated in GameData
    void UpdateDisplay(){
        Debug.Log("Score Updated");
        scoreText.text = "Score: " + GameData.instanceRef.TotalScore;
        healthText.text = "Health: " + GameData.instanceRef.Health;
    }
    
} //end class
```

###Fall 18 Version with Public Variables 

```java
//code updated 4/12/18 11:00am
using System.Collections;
using System.Collections.Generic;
using UnityEngine;
using UnityEngine.UI;
using UnityEngine.Events;

public class PlayerStatsDisplay : MonoBehaviour {

    public Text scoreText, healthText;

	// Use this for initialization
	void Start () {
       //use Inspector to create connection between script and gameObject components
        GameData.instanceRef.onPlayerDataUpdate.AddListener(UpdateDisplay);
        UpdateDisplay(); //call once to set initial values
    }

    //Executed each time score / heath is updated in GameData
    void UpdateDisplay(){
        Debug.Log("Score Updated");
        scoreText.text = "Score: " + GameData.instanceRef.TotalScore;
        healthText.text = "Health: " + GameData.instanceRef.Health;
    }
    
} //end class
```

