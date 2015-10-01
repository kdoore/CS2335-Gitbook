#UI-Panel: Toggle Visibility

###Buttons to control UI Panels.
In the previous section, we also have added a UI-Panel gameObject to our canvas.  Then we used it as a container in the hierarchy to 'hold' our 3 button UI elements. We want to create a different button that can control the visibility of the UI-Panel:  btnPanel and we need to write a custom method in our MenuScript class that will toggle the panel visibility when the user clicks the button. 

The UI Panel can not be directly referenced in our C# scripts.  In order to use the panel to control visibility of itself and it's child elements, we need to add a ``Component`` to the Panel gameObject.  CanvasGroup is the component that we need to add, it will allow us to control the visibility of our panel-UI element from our custom C# script.  Here's a link to the Unity Documentation on [CanvasGroup](http://docs.unity3d.com/Manual/class-CanvasGroup.html)

