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