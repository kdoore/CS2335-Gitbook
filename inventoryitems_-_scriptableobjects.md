# InventoryItems

There are several approaches for creating inventory items.

### Scriptable Objects

Creating ScriptableObjects allows for us to create complex data-type that can be saved by the Unity Engine separate from any gameObject and scene.  For example will allow us to create InventoryItems that we can use across a variety of scenes, each instance of an InventoryItem that we create is stored as a separate project asset. When we use a ScriptableObject in our code, all object references to that ScriptableObject will refer the same object instance, rather than creating new copies of the scriptableObject instances.

```
using UnityEngine;

[System.Serializable]
public class InventoryItem : ScriptableObject
{
    public PickUp.PickUpType pickupType;
    public Sprite Sprite;
    public Vector3 Scale;
    public string ItemName;
    public int Cost;
    public int Strength;
    public int Defense;
}
```

The following class allows us to create InventoryItems in as an Asset. Scriptable Objects can be saved independently of any particular gameObject and are not associated with a scene.  This code must be placed in a folder named Editor within the Assets/Scripts/ folder. The code below is an editor script, it allows us to add functionality to the Unity Editor, this allows us to create and manipulate

```
using UnityEngine;
using UnityEditor;

public class InventoryItemManager : MonoBehaviour {

    [MenuItem("Assets/Create/InventoryItem")]
    public static void CreateAsset()
    {

        InventoryItem inventoryItem = ScriptableObject.CreateInstance<InventoryItem>();

        //Create a .asset file for our new object and save it
        AssetDatabase.CreateAsset(inventoryItem, "Assets/newInventoryItem.asset");
        AssetDatabase.SaveAssets();

        //Now switch the inspector to our new object
        EditorUtility.FocusProjectWindow();
        Selection.activeObject = inventoryItem;
        //CustomAssetUtility.CreateAsset<InventoryItem>();
    }
}
```

### Scriptable Object Factory Unity Plugin

Lior Tal has created a Unity plugin that makes it easy to create scriptable object instances.  His website below provides a link to his github account where he provides a download link to the Unity Plugin.  Import this plug-in into a project and you'll be able to place any scriptableObject instance of code in the


[ScriptableObject Factory - box.com](https://utdallas.box.com/shared/static/tjdlm45kuo46dm5rsfcw641mdtpky33s.unitypackage)








