Dialog Prefab Packages:


1. SimpleDialogPrefab - to be used with SimpleDialog.cs - Uses a List< ConversationEntry > for dialog items.




### Dialog Prefab with Image
 Use with [DialogManager.cs](/conversation-scriptable-objects/dialogmanagerconvlist.md) script.  Uses ScriptableObject Factory, and custom class Conversation.cs which contains List< ConversationEntry > Conversation Scriptable Objects are saved as project assets.  

Import Pagkage: [DialogPrefabw_Image](https://utdallas.box.com/shared/static/u9wf0o3s6zcf3bs9wdtxkr15c313rh2i.unitypackage)

Contents:   DialogPrefabw_Image prefab that has been modifed to include display of a speaker image with each ConversationEntry.  
Includes Conversation.cs script 

Required: Import **[ScriptableObject Factory Package](/scriptable-object-factory.md)**

Required: **Add the DialogManager.cs script** to the top level prefab - see image below.

![](/assets/Screen Shot 2019-11-06 at 12.02.19 PM.png)