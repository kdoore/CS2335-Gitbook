#ScriptableObject Factory 

####Links to Scriptable Object Factory - Unity Package  

[Scriptable Object Factory - Article - Tal Lior](http://www.li0rtal.com/unity-scriptableobject-factory/)

[**ScriptableObjectFactory** Unity Package Download Link - Dropbox](https://www.dropbox.com/s/vdvf5si4go3jnb5/ScriptableObjectFactory.unitypackage)

[**ScriptableObjectFactory** Link - Box.com](https://utdallas.box.com/s/tjdlm45kuo46dm5rsfcw641mdtpky33s)


Any class that inherits from **ScriptableObject** will now show up as an option in the **Project Assets Menu**: 

![](/assets/Screen Shot 2019-03-11 at 3.39.21 PM.png)

The image below shows that the Scriptable Object Factory places a new folder: Editor, with 2 scripts: ScriptableObjectFactory.cs, ScriptableObjectWindow.cs.  

![](/assets/Screen Shot 2019-03-11 at 2.20.16 PM.png)

- Add a public instance of the Scriptable Object to a custom script that will be attached to a gameObject in a scene.  In our case this is the DialogManager.cs script.
  
     ```java
public class DialogManager : MonoBehaviour {

     public ConversationList convList; //attach scriptable object in Inspector
     
     //more class definition code below
```

- Use inspector to select a ConversationList scriptableObject from your project's assets, to populate the convList variable defined above.

>Scriptable Objects are amazing data containers. They don't need to be attached to a GameObject in a scene. They can be saved as assets in our project. Most often, they are used as assets which are only meant to store data, but can also be used to help serialize objects and can be instantiated in our scenes.  [Adam Buckner - Official Unity Tutorial](/Adam Buckner)

[Scriptable Objects - Unity Manual, Scripting API](https://docs.unity3d.com/Manual/class-ScriptableObject.html?_ga=2.21799608.2060655022.1537129337-1484586345.1534795685)

ScriptableObject is a class that allows you to store large quantities of shared data independent from script instances. 

A class you can derive from if you want to create objects that don't need to be attached to game objects.  This is most useful for assets which are only meant to store data.




