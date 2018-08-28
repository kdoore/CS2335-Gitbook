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







    
