# Conversation

```
public void NextConversationEntry(){
		Debug.Log ("Next conversation entry" + convIndex);
		convIndex++;
		if (managerRef.myConversation.ConversationLines.Length > convIndex) {
			cText.text = managerRef.myConversation.ConversationLines[convIndex].ConversationText;
			cImage.sprite = managerRef.myConversation.ConversationLines[convIndex].DisplayImg;
		} else {
			convIndex = managerRef.myConversation.ConversationLines.Length - 1;
		}
	}

```

###State Code For Conversation Elements
BeginState, load a Conversation scriptableObject that is located in the Resources folder within Assets.
```
/// load conversation asset
		/// 
		conv2=Resources.Load("Conversation2", typeof(Conversation)) as Conversation;
		if (conv2 == null) {
			Debug.Log ("Not loaded");
		} else {
			Debug.Log ("Conversation Asset Loaded, first line of text: ");
			Debug.Log (conv2.ConversationLines [0].ConversationText);
		}

		cText = GameObject.Find ("ConversationText").GetComponent<Text> ();
		cName = GameObject.Find ("ConversationName").GetComponent<Text> ();
		cImage = GameObject.Find ("ConversationImg").GetComponent<Image> ();

		convBtn = GameObject.Find ("ConvButton").GetComponent<Button> ();
		convBtn.onClick.AddListener (getNextConversation);

		convBackBtn = GameObject.Find ("ConvButtonBack").GetComponent<Button> ();
		convBackBtn.onClick.AddListener (getPrevConversation);

		/////show first conversation at start
		if(conv2.ConversationLines.Length != 0){
			convIndex = Utilities.GetConversationEntry (conv2, cName, cText, cImage, convIndex);
		}

	}
	/// <summary>
	/// Gets the conversation entry.
	/// </summary>
	public void getNextConversation(){
		Debug.Log ("Next conversation entry" + convIndex);
		convIndex = Utilities.GetConversationEntry (conv2, cName, cText, cImage, convIndex);
	}
	/// <summary>
	/// Gets the previous conversation.
	/// </summary>
	public void getPrevConversation(){
		Debug.Log ("Prev conversation entry" + convIndex);
		convIndex = Utilities.GetConversationEntry (conv2, cName, cText, cImage, convIndex-2);
	}

```

###Utilities Static Functions:
```

using UnityEngine;
using UnityEngine.UI;
using System.Collections;

public static class Utilities
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

	public static int GetConversationEntry (Conversation _conversation, Text _name, Text _text, Image _image, int _index)
	{
		// how long is our array?
		int numEntries = _conversation.ConversationLines.Length;

		_index++;  

		//check ranges, constrain to  _index >= 0 && _index <= numEntries-1

		_index = (_index < 0) ? 0 : _index; 
		_index = (_index >= numEntries) ? numEntries - 1 : _index; 

		//set objectRefs to values
		_name.text = _conversation.ConversationLines [_index].SpeakingCharacterName;
		_text.text = _conversation.ConversationLines [_index].ConversationText;
		_image.sprite = _conversation.ConversationLines [_index].DisplayImg;

		Debug.Log ("Next conversation entry" + _index + " name: " + _name.text);

		return _index;
	}
}


```

![](Screenshot 2016-03-10 17.49.12.png)