# Conversation Scriptable Objects

### Scriptable Object Factory Unity Package

Lior Tal has created a Unity package that makes it easy to create scriptable object instances.  His website below provides a link to his github account where he provides a download link to the Unity Plugin.  

**Import this Unity Package into a Unity project** and you'll be able to create any scriptableObject using the project-panel's context menu.

[http://www.tallior.com/unity-scriptableobject-factory/](http://www.tallior.com/unity-scriptableobject-factory/)



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

public class ConversationList : ScriptableObject
{
    public List<ConversationEntry> Conversation;
}
```

### Create a Conversation Asset

To create an instance of a Conversation asset, we right click in the project panel, or right click on the assets folder and select: create -&gt; Conversation, where our custom asset: Conversation, now shows up as an option at the bottom of the menu.  After clicking on Conversation, we now have a new item in our Assets panel, we should give it a unique name so we can reference it in our code.  I've named the example one: `ConversationList_CatScene1`.

![](Screenshot 2016-03-08 14.18.18.png)

![](Screenshot 2016-03-08 14.35.40.png)



