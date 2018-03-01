# Static Classes

"A static class is basically the same as a non-static class, but there is one difference: a static class cannot be instantiated. In other words, you cannot use the new keyword to create a variable of the class type. Because there is no instance variable, you access the members of a static class by using the class name itself."
[MSDN Reference - C# Programming Guide](https://msdn.microsoft.com/en-us/library/79b3xss3.aspx)

The Utility class below can be used to provide utility methods to hide and show Canvas groups, since this code will be used quite frequently throughout our project.

```C#
using UnityEngine;
using UnityEngine.UI;
using System.Collections;

public class Utility  {

	public static void ShowCG( CanvasGroup cg){
		cg.alpha = 1;
		cg.blocksRaycasts = true;
		cg.interactable = true;
	}

	public static void HideCG( CanvasGroup cg){
		cg.alpha = 0;
		cg.blocksRaycasts = false;
		cg.interactable = false;
	}

}

//In another class we can use this by: 
CanvasGroup cg = GameObject.Find ("TxtPanel1").GetComponent<CanvasGroup>();
Utility.HideCG (cg);
```
