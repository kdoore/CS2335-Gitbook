#Basic Buttons

In the previous section, we created a simple dynamic text element that changed from it's default value ( displayed before playing the scene) to it's initialized value (set in Start( ) ) of the Controller.cs script. In this section, we'll add buttons to change the text.

#Create UI- Button

1. Add a UI-Button GameObject to your scene.
2. Configure the UI-Button
    - Give the Button gameObject a unique name: such as: 'Button1'
    - Set the Rect-Transform of the Button so it is anchored to the bottom of it's parent, the Canvas gameObject.
    - Set the Highlighted color of the Button component so that it's different than the Normal color. - in playmode, verify the mouse-over behavior shows the highlight color.
    -  change the Button's child, Text, so it provides instructions for use, such as 'Click Here'.
    
# Update the Controller Script
1.  Create an object reference variable for the Button component.



```java
Button button1; //declare obj-reference variable
```

2.  Connect the variable with the GameObject's Button component in Start()

```java
button1 = GameObject.Find("Button1").GetComponent<Button>();
```

3.  Write a public custom method (function) that will be executed when the Button is clicked:



```java
public void DoSomething(){
    displayText.text = "Hey Button1 was clicked";
}
```

4.  In Start( ), after finding the Button component, add the following code so the DoSomething( ) method is added as a listener to the Button's onClick event.


```java

  button1.onClick.AddListener(DoSomething);

```


###Updated Controller.cs Script

```java
using System.Collections;
using System.Collections.Generic;
using UnityEngine;
using UnityEngine.UI;

public class Controller : MonoBehaviour {

   public int score = 5;   //declare and initialize

    /// <summary>
    /// We want to modify the text field of the Text component 
    /// </summary>
    Text displayText; //can connect wiht a Text Component

    GameObject TextGameObject; //can connect with a GameObject

    Button button1;

	// Use this for initialization
	private void Start () {
        score = 10;
        Debug.Log("score " + score);
        TextGameObject = GameObject.Find("DisplayText");
        displayText = TextGameObject.GetComponent< Text >(); //Generics <T> is a placeholder

        displayText = GameObject.Find("DisplayText").GetComponent<Text>();
        displayText.text = "HELLO Today is not Tuesday";

        button1 = GameObject.Find("Button1").GetComponent<Button>();
        button1.onClick.AddListener(DoSomething);

	}
	
	// Update is called once per frame
	private void Update () {
		
	}

    //custom function / method to be executed when  Button1 is clicked
    public void DoSomething(   ){
        displayText.text = "Yipee you clicked Button1 !!!!!!!!!";
    }

}



```

#FULL CODE For Controller.cs 



```java
using System.Collections;
using System.Collections.Generic;
using UnityEngine;
using UnityEngine.UI; //make sure to include this for UI behaviour scripts

public class Controller : MonoBehaviour {

   public int score = 5;   //declare and initialize

    /// <summary>
    /// We want to modify the text field of the Text component 
    /// </summary>
    Text displayText, toggleBtnText; //can connect wiht a Text Component

    Button button1,openDialogBtn;
    CanvasGroup dialogPanelCG;

	// Use this for initialization
	private void Start () {
        score = 10;
        Debug.Log("score " + score);

        //connect obj-ref variables with GameObject components: 
        displayText = GameObject.Find("DisplayText").GetComponent<Text>();
        displayText.text = "HELLO Today is not Tuesday";

        toggleBtnText = GameObject.Find("ToggleBtnText").GetComponent<Text>();
        toggleBtnText.text = "Hide Dialog"; //initialize Button's text 

        //connect button1 obj-ref variable with Button1's Button Component
        button1 = GameObject.Find("Button1").GetComponent<Button>();
        button1.onClick.AddListener(DoSomething); //execute DoSomething when button is clicked

        dialogPanelCG = GameObject.Find("DialogPanel").GetComponent<CanvasGroup>();

        openDialogBtn = GameObject.Find("OpenDialogButton").GetComponent<Button>();
        openDialogBtn.onClick.AddListener(ToggleDialogVisibility);
        ShowCG(dialogPanelCG);

	}
	
	
    //custom function / method to be executed when  Button1 is clicked
    public void DoSomething(   ){
        displayText.text = "Yipee you clicked Button1 !!!!!!!!!";
    }

    //OpenDialogButton calls this method when onClick is executed to
    //toggle visiblity of the DialogPanel - using CanvasGroup for behaviour
    public void ToggleDialogVisibility(){
        if(dialogPanelCG.alpha >=.5 ){
            HideCG(dialogPanelCG);
            toggleBtnText.text = "Show Dialog";
        }else{
            ShowCG(dialogPanelCG);
            toggleBtnText.text = "Hide Dialog";
        }

    }

    /// <summary>
    /// Toggle visibility/interactivity of canvas group & children to ON
    /// </summary>
    /// <param name="cg">Cg.</param>
    /// This will be moved to a Utility Class
    void ShowCG( CanvasGroup cg){
        cg.alpha = 1;
        cg.blocksRaycasts = true;
        cg.interactable = true;
    }
    /// <summary>
    ///toggle visibility/interactivity of canvas group & children to OFF
    /// </summary>
    /// <param name="cg">Cg.</param>
    /// This will be moved to a Utility Class
    void HideCG(CanvasGroup cg)
    {
        cg.alpha = 0;
        cg.blocksRaycasts = false;
        cg.interactable = false;
    }

}

```



    
