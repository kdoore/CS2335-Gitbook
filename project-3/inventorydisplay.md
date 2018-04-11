#Inventory Display

Use the following code for the Inventory Display functionality

Inventory Display - Unity Package
[https://utdallas.box.com/v/InventoryDisplay](https://utdallas.box.com/v/InventoryDisplay)

The code below is included in the package file.


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
	
	// Update is called once per frame
	void UpdateDisplay () {
        inventory = GameData.instanceRef.inventory;
        //get each item and update the display
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

