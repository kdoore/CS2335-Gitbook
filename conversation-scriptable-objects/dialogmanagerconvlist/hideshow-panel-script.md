#Hide_Show_Panel Script

The following script can be put on any panel to control whether the panel is hidden when the scene starts.

This script can be used on the DecisionPanel which controls buttons for changing scenes. A checkbox in the inspector provides an option to have the panel start in open state when the scene loads.

**The Panel must have a CanvasGroup Component. ** 

When working with [SimpleDialog](/simple-dialog-prefab.md), or [DialogManager](/conversation-scriptable-objects/dialogmanagerconvlist.md), this DecisionPanel should be set as the Next Panel To Open in the SimpleDialog or DialogManager script.


```java

using System.Collections;
using System.Collections.Generic;
using UnityEngine;
using UnityEngine.UI;

public class Hide_Show_Panel : MonoBehaviour {

    CanvasGroup panelCG;
    public bool showOnStart = false;
    public Button closeButton; //clicking will hide
    public Button openButton; //clicking will show

    // Use this for initialization
    void Start()
    {
        panelCG = GetComponent<CanvasGroup>();
        if (!showOnStart)
        {
            HidePanel();
        }
        if (closeButton != null)
        {
            closeButton.onClick.AddListener(HidePanel);
        }
        if (openButton != null)
        {
            closeButton.onClick.AddListener(ShowPanel);
        }

    }

    public void ShowPanel()
    {
        Utility.ShowCG(panelCG);
    }

    public void HidePanel()
    {
        Utility.HideCG(panelCG);
    }
}


```

