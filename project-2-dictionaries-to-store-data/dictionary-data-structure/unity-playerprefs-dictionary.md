#Unity PlayerPref Dictionaries:

[Unity Scripting API - PlayerPrefs](https://docs.unity3d.com/ScriptReference/PlayerPrefs.html)

Unity provides easy access to 3 built-in Dictionary data structures: Int, Float, String

Links:  
[Unity Store HighScore](https://unity3d.com/learn/tutorials/topics/scripting/high-score-playerprefs)

Example:  [PlayerPrefs.SetInt](https://docs.unity3d.com/ScriptReference/PlayerPrefs.SetInt.html)

```java
 // This function could be extended easily to handle any additional data we wanted to store in our PlayerProgress object
    private void SavePlayerProgress()
    {
        // Save the value playerProgress.highestScore to PlayerPrefs, with a key of "highestScore"
        PlayerPrefs.SetInt("highestScore", playerProgress.highestScore);
    }
```


Example:  [PlayerPrefs.SetFloat](https://docs.unity3d.com/ScriptReference/PlayerPrefs.SetFloat.html)
```java
var myVariable : float;
 
PlayerPrefs.SetFloat("Player Score", 10.0);
 
myVariable = PlayerPrefs.GetFloat("Player Score");
 
print (myVariable);
```

Example:  [PlayerPrefs.SetString](https://docs.unity3d.com/ScriptReference/PlayerPrefs.GetString.html)


```java

using UnityEngine;
using UnityEngine.UI;

public class PlayerPrefsDeleteAllExample : MonoBehaviour
{
    string m_PlayerName;

    void Start()
    {
        //Fetch the PlayerPref settings
        SetText();
    }

    void SetText()
    {
        //Fetch name (string) from the PlayerPrefs (set these Playerprefs in another script). If no string exists, the default is "No Name"
        m_PlayerName = PlayerPrefs.GetString("Name", "No Name");
    }

    void OnGUI()
    {
        //Fetch the PlayerPrefs settings and output them to the screen using Labels
        GUI.Label(new Rect(50, 50, 200, 30), "Name : " + m_PlayerName);
    }
}

```

