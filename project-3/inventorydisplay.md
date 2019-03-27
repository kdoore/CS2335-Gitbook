#Inventory Display

Use the following code for the Inventory Display functionality


The code below shows the parts of this code that you will need to customize, to match with the PickupType enums that you've defined in your PickUp script.  You need to make sure you have 3 different types of good prefabs in your game, these will be displayed in this inventory display.

###Prefab PickUp - Set the type variable in the inspector
When creating the prefabs, make sure to set the PickUp component's `type` value in the inspector, and select the prefab 'apply' button to apply, if you are modifying a prefab instance instead of the actual prefab in your assets folder. In the image below, the PickUp type has been set to Crystal, the drop-down list of values are determined by the PickUpType enums. This `type` variable is used to track the type of item stored and displayed in your inventory.

![](/assets/Screen Shot 2018-04-12 at 10.30.46 AM.png)

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
                starActive=true;
                }
                itemText = starPanelCG.gameObject.GetComponentInChildren<Text>();
                itemText.text = value.ToString();
                break;

            case PickupType.crystal:
                if(crystalActive==false){
                    Utility.ShowCG(crystalPanelCG);
                    crystalAtive=true;
                }

                itemText = crystalPanelCG.gameObject.GetComponentInChildren<Text>();
                itemText.text = value.ToString();
                break;

            case PickupType.gem:
                if(gemActive==false){
                    Utility.ShowCG(gemPanelCG);
                    gemActive=true;
                }
               
                itemText = gemPanelCG.gameObject.GetComponentInChildren<Text>();
                itemText.text = value.ToString();
                break;

        }
    }

}

```

