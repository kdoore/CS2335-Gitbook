##Code Sept26 -F18



```java

using System.Collections;
using System.Collections.Generic;
using UnityEngine;
using UnityEngine.UI;

public class SimpleDialog : MonoBehaviour
{

    CanvasGroup canvasGroup;
    Button nextBtn;
    Text dialogText;

    public Button openBtn; //make connection in inspector
    public CanvasGroup nextPanelToOpen;

    [TextArea]
    public List<string> dialogList = new List<string>();

    Queue<string> dialogQueue = new Queue<string>();


    // Use this for initialization
    void Start()
    {
        canvasGroup = GetComponent<CanvasGroup>();

        nextBtn = GetComponentInChildren<Button>();
        nextBtn.onClick.AddListener(GetNextDialog);

        openBtn.onClick.AddListener(ShowPanel);

        dialogText = GetComponentInChildren<Text>();

        foreach (string item in dialogList)
        { //put dialog in the queue
            dialogQueue.Enqueue(item);
        }
        if (dialogQueue.Count > 0)
        {
            dialogText.text = dialogQueue.Dequeue();
        }

        Utility.HideCG(canvasGroup);
    }

    public void GetNextDialog()
    {
        if (dialogQueue.Count > 0)
        {
            dialogText.text = dialogQueue.Dequeue();
        }
        else
        {////This is where the event happens - the dialog is done
            Utility.HideCG(canvasGroup);
            Utility.ShowCG(nextPanelToOpen);
        }
    }

    public void ShowPanel()
    {
        Utility.ShowCG(canvasGroup);
    }
}
```

