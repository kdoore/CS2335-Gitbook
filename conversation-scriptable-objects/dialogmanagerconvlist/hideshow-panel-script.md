#Hide_Show_Panel Script

The following script can be put on any panel to control whether the panel is hidden when the scene starts.

This script can be used on the DecisionPanel which controls buttons for changing scenes. A checkbox in the inspector provides an option to have the panel start in open state when the scene loads.

The Panel must have a CanvasGroup.  

When working with [DialogManager](/conversation-scriptable-objects/dialogmanagerconvlist.md), this DecisionPanel should be set as the Next Panel To Open in the DialogManager script.



```java

using System.Collections;
using System.Collections.Generic;
using UnityEngine;
using UnityEngine.UI;


/// <summary>
/// Hide show panel.
/// Checkbox controls whether panel is shown on scene start
/// Can call ShowPanel from some other script or button
/// Panel must have a CanvasGroup
/// </summary>
public class Hide_Show_Panel : MonoBehaviour {

    CanvasGroup panelCG;
    public bool showOnStart=false;

	// Use this for initialization
	void Start () {
        panelCG = GetComponent<CanvasGroup>();
        if( ! showOnStart){
            HidePanel();
        }
	}
	
    public void ShowPanel(){
        Utility.HideCG(panelCG);
    }

    public void HidePanel(){
        Utility.HideCG(panelCG);
    }
	
}

```

