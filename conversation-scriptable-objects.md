# Conversation Scriptable Objects
Scriptable Objects provide an easy way to create and populate custom data elements. In this course, will use scriptable objects to store dialog data and inventory items.

In the image below, we've created a ConversationList scriptableObject, to permanently store date associated with a Conversation.

![](/assets/Screen Shot 2019-03-11 at 2.19.08 PM.png)

There are several steps required when creating a new type of scriptable object. Detailed example provided below using our Dialog Manager example: 
    -  **Define a custom class** (or struct) that has variables for each of the items you'll have associated with each  scriptable object.  This class must be defined using the `[System.Serializable] `attribute:  See details in full code below.
        - `Class ConversationEntry ` [[See code below]](https://kdoore.gitbooks.io/cs-2335/content/conversation-scriptable-objects.html#class-conversationentry)
        
-  Define a custom class that inherits from the Scriptable Object class. See details in full code below.

        - `public class ConversationList : ScriptableObject` [[See code below]](https://kdoore.gitbooks.io/cs-2335/content/conversation-scriptable-objects.html#class-conversationlist)


-  Define a collection data-structure, within this class the  to hold a collection of the above defined composite elements. This is code within ConversationList class. 
        - `public List<ConversationEntry> Conversation`

### Install the Scriptable Object Factory - Unity Package  

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


###Class ConversationEntry

```java
using UnityEngine;
using System.Collections;

[System.Serializable]  //attribute so this custom class can be displayed as an item in the inspector
public class ConversationEntry  {

    public string SpeakingCharacterName;
    public string ConversationText;
    public Sprite DisplayImg;
}
```

###Class ConversationList
This class

```java
using UnityEngine;
using System.Collections;
using System.Collections.Generic;

public class ConversationList : ScriptableObject
{
    public List<ConversationEntry> Conversation;
}
```

# Using the ConversationList

###Use in DialogManager Class
Use as a public object-reference variable in a custom class, like DialogManager, that inherits from MonoBehaviour: The scriptable object instance will be connected in the inspector panel. 

```java
public class DialogManager : MonoBehaviour {

     public ConversationList convList; //attach scriptable object in Inspector
     
     //more class definition code below
```

### Create a ConversationList Asset

To create an instance of a Conversation asset, we right click in the project panel, or right click on the assets folder and select the bottom-most menu option: create -&gt; ScriptableObject, where our custom asset: Conversation, now shows up as an option at the bottom of the menu.  After clicking on Conversation, we now have a new item in our Assets panel, we should give it a unique name so we can reference it in our code.  I've named the example one: `ConversationList_CatScene1`.

![](/assets/Screen Shot 2018-09-16 at 3.50.41 PM.png)

![](/assets/Screen Shot 2018-09-16 at 2.48.30 PM.png)

###Select ScriptableObject Instance in Inspector
The image below shows that the public ConversationList variable: convList has been set in the inspector by selecting the circle-icon to the right of the ConvList item.


![](/assets/Screen Shot 2018-09-16 at 3.56.35 PM.png) 

### Download Scriptable Object Factory Unity Package

Lior Tal has created a Unity package that makes it easy to create scriptable object instances.  His website below provides a link to his github account where he provides a download link to the Unity Plugin.  I have included these files in the Dialog Unity_Package

**Import this Unity Package into a Unity project** and you'll be able to create any scriptableObject using the project-panel's context menu.

[http://www.tallior.com/unity-scriptableobject-factory/](http://www.tallior.com/unity-scriptableobject-factory/)


**Resource Links: **
[Scriptableobject-factory - Unity Package](http://www.tallior.com/unity-scriptableobject-factory/)


[Scriptable Object Article](http://ivanozanchetta.com/gamedev/unity3d/unity-serialization-behind-scriptableobject/)



