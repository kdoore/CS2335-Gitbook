# Utility - Static Class

We can create a static class: Utility, that has static methods we can call from any script without creating an object instance of the class.  We have been using static methods of the GameObject class, such as: GameObject.Find( ). Static methods provide a convenient way to create methods that we'll use across a series of classes, for functionality that is not specific to a single class.  A perfect example of this type of method is that we often want to toggle visibility of a UI-Panel, and we can do this by adding a Canvas Group component to the UI gameObject.

Notice, the class is marked as static and each method is also marked as public static.  This means we can use the method in another class with the following syntax:

```C#

Utility.showPanel( someCanvasGroup );

```

```C#

using UnityEngine;
using UnityEngine.UI;
using System.Collections;

public static class Utility
{

	public static void ShowPanel (CanvasGroup cg)
	{
		cg.alpha = 1;
		cg.blocksRaycasts = true;
		cg.interactable = true;
	}

	public static void HidePanel (CanvasGroup cg)
	{
		cg.alpha = 0;
		cg.blocksRaycasts = false;
		cg.interactable = false;
	}

	public static int GetConversationEntry (Conversation conversation, Text name, Text text, Image image, int index)
	{
		// how long is our array?
		int numEntries = conversation.ConversationLines.Length;

		index++;  ///increment to move to next element

		//check ranges, constrain to  _index >= 0 && _index <= numEntries-1
        index = (index < 0) ? 0 : index; //ternary operator ?
		index = (index >= numEntries) ? numEntries - 1 : index; 


		//set objectRefs to values
		name.text = conversation.ConversationLines [index].SpeakingCharacterName;
		text.text = conversation.ConversationLines [index].ConversationText;
		image.sprite = conversation.ConversationLines [index].DisplayImg;

		Debug.Log ("Next conversation entry" + index + " name: " + name.text);

		return index;
	}
}
```
