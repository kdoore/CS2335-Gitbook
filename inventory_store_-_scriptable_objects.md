# Inventory Display

In this section, we create a set of gameObject panels that we can use to show the inventory of items that the player has collected during the game.

The first step to creating an inventory display is to create a single UI panel that can function as the template for displaying your inventory items.  This can be used to create a prefab that can be customized to display each type of collectable object.

The image below shows the panels are contained within a larger-panel, and that parent panel has a Layout-Component: Horizontal Layout Group

![](Screenshot 2016-11-14 16.12.18.png)

The parent panel has several child panels, each of which contain a UI-image and UI-text.  The Horizontal Layout Group.  The logic for displaying the inventory items is:

1. Create and resize a UI-panel that can be opened and closed, create a button that can show / hide this panel.  Within this parent UI panel, we'll place child objects that are prefabs to display each type of inventory item, along with a text item to show how many of the items we have.

    - Create individual Prefabs with: a panel, image, and text for each item that will be in the inventory.  
    - Add a horizontal layout component to the parent UI-panel.

![](Screenshot 2016-11-14 14.05.17.png)

### Inventory Display:

```java

using System.Collections;
using System.Collections.Generic;
using UnityEngine;
using UnityEngine.UI;

public class InventoryDisplay : MonoBehaviour {

    Button inventoryDisplayBtn;
    CanvasGroup panelCG, starPanelCG, gemPanelCG, crystalPanelCG;
    Dictionary<PickupType, int> inventory;
    bool starActive, crystalActive, gemActive;
	// Use this for initialization
	void Start () {
        inventoryDisplayBtn = GameObject.Find("InventoryDisplayBtn").GetComponent<Button>();
        inventoryDisplayBtn.onClick.AddListener(ShowHideInventory);

        panelCG = GameObject.Find("InventoryDisplayPanel").GetComponent<CanvasGroup>();
        starPanelCG = GameObject.Find("StarPanel").GetComponent<CanvasGroup>();
        crystalPanelCG = GameObject.Find("CrystalPanel").GetComponent<CanvasGroup>();
        gemPanelCG = GameObject.Find("GemPanel").GetComponent<CanvasGroup>();
        Utility.HideCG(starPanelCG);
        Utility.HideCG(gemPanelCG);
        Utility.HideCG(crystalPanelCG);
        Utility.HideCG(panelCG);
        starActive = false;
        gemActive = false;
        crystalActive = false;

        GameData.instanceRef.onPlayerDataUpdate.AddListener(UpdateDisplay);
    }

    public void ShowHideInventory(){
        if(panelCG.alpha > 0){ //currently visible
            Utility.HideCG(panelCG);
        }else{
            Utility.ShowCG(panelCG);
            Debug.Log("ShowPanel");
        }

    }
	
	// Update Display is executed when GameData's PlayerDataUpdate UnityEvent is Invoked
	void UpdateDisplay () {
        inventory = GameData.instanceRef.inventory; //get inventory

        foreach (var item in inventory)
        {
            updateUI(item.Key, item.Value);
        }   
	}

    void updateUI(PickupType type, int value){
        Text itemText;
        switch (type)
        {
            case PickupType.star:
                if (starActive == false) { 
                Utility.ShowCG(starPanelCG);
                }
                itemText = starPanelCG.gameObject.GetComponentInChildren<Text>();
                itemText.text = value.ToString();
                break;

            case PickupType.crystal:
                if(crystalActive==false){
                    Utility.ShowCG(crystalPanelCG);
                }

                itemText = crystalPanelCG.gameObject.GetComponentInChildren<Text>();
                itemText.text = value.ToString();
                break;

            case PickupType.gem:
                if(gemActive==false){
                    Utility.ShowCG(gemPanelCG);
                }
               
                itemText = gemPanelCG.gameObject.GetComponentInChildren<Text>();
                itemText.text = value.ToString();
                break;

        }
    }

}


```



