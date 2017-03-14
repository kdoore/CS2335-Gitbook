# Utility - Static Class

We created a static class: [Utility](https://kdoore.gitbooks.io/cs-2335/content/utility_-_static_class.html#utility---static-class), that has static methods we can call from any script without creating an object instance of the class.  We have been using static methods of the GameObject class, such as: GameObject.Find( ). Static methods provide a convenient way to create methods that we'll use across a series of classes, for functionality that is not specific to a single class.  A perfect example of this type of method is that we often want to toggle visibility of a UI-Panel, and we can do this by adding a Canvas Group component to the UI gameObject.

Notice, the class is marked as static and each method is also marked as public static.  This means we can use the method in another class with the following syntax:

###Example of using Utility Static Method: showPanel( )

```C#
//toggle canvas-group visibility
CanvasGroup someCG = GameObject.Find("SomePanel").GetComponent<CanvasGroup>();
Utility.ShowCG( someCG );

```

###Utility.cs Class
```C#

using UnityEngine;
using UnityEngine.UI;
using System.Collections;

public static class Utility
{

	public static void ShowCG (CanvasGroup cg)
	{
		cg.alpha = 1;
		cg.blocksRaycasts = true;
		cg.interactable = true;
	}

	public static void HideCG (CanvasGroup cg)
	{
		cg.alpha = 0;
		cg.blocksRaycasts = false;
		cg.interactable = false;
	}

}
```
