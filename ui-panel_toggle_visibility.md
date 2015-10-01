#UI-Panel: Toggle Visibility

###Buttons to control UI Panels.
In the previous section, we also have added a UI-Panel gameObject to our canvas.  Then we used it as a container in the hierarchy to 'hold' our 3 button UI elements. We want to create a different button that can control the visibility of the UI-Panel:  btnPanel and we need to write a custom method in our MenuScript class that will toggle the panel visibility when the user clicks the button. 

The UI Panel can not be directly referenced in our C# scripts.  In order to use the panel to control visibility of it's