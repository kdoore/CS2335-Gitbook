#Timer
The code below is an example of a simple countdown timer.  The code would need modified to be useful in the LevelManager.  


//modified ,based on Code from:[https://www.noob-programmer.com/unity3d/countdown-timer/ ](https://www.noob-programmer.com/unity3d/countdown-timer/)




```java
using System.Collections;
using UnityEngine;
using UnityEngine.UI;

public class CountDown : MonoBehaviour {
  
  public int timeLeft = 60; //Seconds Overall
  public Text countdown; //UI Text Object
  void Start () {
    StartCoroutine("LoseTime");
    Time.timeScale = 1; //Just making sure that the timeScale is right
  }
  void Update () {
    countdown.text = ("" + timeLeft); //Showing the Score on the Canvas
  }
  //Simple Coroutine
  IEnumerator LoseTime()
  {
    while (true) {
      yield return new WaitForSeconds (1);
      timeLeft--; 
    }
  }
  
} //end class
```

