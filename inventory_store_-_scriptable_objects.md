# Inventory Display

In this section, we create a set of gameObject panels that we can use to show the inventory of items that the player has collected during the game.


![](Screenshot 2016-11-14 14.05.17.png)

###Inventory Display: 

```C#
using UnityEngine;
using UnityEngine.UI;
using System.Collections;
using System.Collections.Generic;


public class InventoryDisplay : MonoBehaviour
{

	public List<GameObject> prefabs;
	public List<InventoryItem> items;
	// Use this for initialization
	void Start ()
	{
		//prefabs = new List<GameObject> ();
		for (int i = 0; i < prefabs.Count; i++) {
			prefabs [i].SetActive (false);
		}
		GameData.instanceRef.onPlayerDataUpdate += updateInventoryDisplay;
	}

	public void updateInventoryDisplay (object s, PlayerDataEventArgs e)
	{
		Dictionary<PickupType, int> inventory = GameData.instanceRef.inventory;

		foreach (var item in inventory) {
			int itemTotal = item.Value;
			UpdateUI (itemTotal, item.Key); 
			Debug.Log ("itemTotal " + itemTotal);
		}
	}

	void UpdateUI (int val, PickupType item)
	{
		Text temp;
		switch (item) {

		case PickupType.crystal:
			if (val > 0) {
				prefabs [0].SetActive (true);
				temp = prefabs [0].GetComponentInChildren<Text> ();
				temp.text = string.Format ("{0}", val);
			} else {
				prefabs [0].SetActive (false);
			}

			break;

		case PickupType.star:
			if (val > 0) {
				prefabs [1].SetActive (true);
				temp = prefabs [1].GetComponentInChildren<Text> ();
				temp.text = string.Format ("{0}", val);
			} else {
				prefabs [1].SetActive (false);
			}
			break;

		case PickupType.purpleGem:
			if (val > 0) {
				prefabs [2].SetActive (true);
				temp = prefabs [2].GetComponentInChildren<Text> ();
				temp.text = string.Format ("{0}", val);
			} else {
				prefabs [2].SetActive (false);
			}
			break;

		}
	}

}
```

