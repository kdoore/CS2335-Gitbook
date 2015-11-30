# InventoryItems - ScriptableObjects

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
The following class allows us to create InventoryItems in as an Asset. Scriptable Objects can be saved independently of any particular gameObject.

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