# Animation Controller
The Unity Animation controller is a visual programming language implementation of a state machine to control coordination of animations which can be attached to gameObjects. [Unity Animation Beginner Tutorial](https://unity3d.com/learn/tutorials/modules/beginner/live-training-archive/animate-anything)


###Create an Animation from a series of sprites
To create an animation, we can select a series of sprites and drag them into the scene window.  We are then prompted to save this animation.  This immediately creates several components:

* Animation clip
* Animation controller
* Animator component


In addition, in order to integrate animations with our Unity program, we will probably want to use the following additional items:
  
*  Animator parameters
*  Custom C# controller script

###Animation Clip
In 2D mode, we can create animation clips using sprites.  We can use a series of sprites, where each sprite is in a slightly different position, so that when we show these sprites in a sequential loop, we see animated behavior.  Or, we can use a single sprite, and use the Animation editor to modify some feature of the sprite over time using the dope-sheet or curves editor.

###Animation Controller
The Animation Controller, Mechanim, is a visual interface for creating a state machine to control animations in Unity.  It has the ability to utilize input parameters as event signals, these input parameters provide the connection between the Mechanim state-machine and custom C# scripts.  There are many online tutorial examples that show how to use Mechanim as a state-machine to control game-logic, instead of animations.  The animation controller has a set of nodes that correspond to states, it is easy to add arc - transitions between nodes. Each transition can be configured to use any number of conditions to determine when a transition is exectued to change the sytem's current state.  

###Animation Component
In order to integrate an animation with a gameObject, we need a 2D gameObject that has a Sprite Renderer, then we can add an Animation Component from the component miscellaneous tab.  Then select the empty animator-controller section in the inspector to select an animation controller from your assets folder.  

###Animation Controller Script
To control a animation associated with a gameObject, outside of the Animation Controller, such as user-input, then we'll need a custom C# script that can manage the logic to determine what input events have occurred and whether the input should impact the animation controller state.  Our custom script will send input signals to the Animation Controller State Machine, where the animation controller state machine will consider the current animator state, the input signals, and the defined transition conditions to determine whether to switch to a new animation clip. 

Below is a simple example class: PlayerController.cs which checks for left-right arrow keyboard input, or the fire1 key which maps to the control key on a MAC.  In Fixed-Update, the code checks for input, then uses if-else statement blocks to determine what value to send to the Animator component that is attached to the gameObject.



###Unity 2D Animation Tutorials:

[Introduction to Unity Animation: RayWenderlich.com ](https://www.raywenderlich.com/116652/introduction-unity-animation-system)

[Introduction to Unity 2D](https://www.raywenderlich.com/115688/introduction-unity-2d)

 John Stejskal, [IndieGameBuzz 3-Part tutorial](http://indiegamebuzz.com/create-2d-sprite-based-animation-states-in-unity3d/)
 
[rm2kdev:](https://www.youtube.com/watch?v=TU6wflRqT5Q) Unity Animator Tutorial  with Player Sprite Sheet

