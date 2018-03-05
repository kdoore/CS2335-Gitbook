#Simple Dialog Prefab

How can we create a very simple dialog system in Unity?  
We should try to make something that will work as a prefab, so we can use it in any scene, as many times as we'd like. 
- We need to include
    - UI-panel as a container with a CanvasGroup component   so we can toggle it's visibiltiy
    - UI-Text, a child of the UI-panel, this will display the text
    - UI-Button, a child of the UI-panel, this will allow us to advance through the dialog items
    - A custom script component- attached to the UI-Panel
    will have logic for the dialog system.
      -Script must use a public List of strings that can be populated in the inspector, with new dialog each time the panel-prefab is used.  

 ###Prefab GameObject: Hierarchy     
 The image below shows the Hierarchy panel structure of these gameObjects.  The DialogController.cs script is attached to the top-level panel object.  The panel must also have a CanvasGroup component attached. To add a CanvasGroup component, in the Inspector panel: Add Component>Layout > CanvasGroup
 
![](/assets/Screen Shot 2018-03-01 at 4.06.32 PM.png)

The DialogText is anchored to 4 corners of its' parent, the SimpleDialogPanel.  The NextButton is anchored to the bottom  corner of the panel (not shown here).

![](/assets/Screen Shot 2018-03-01 at 4.26.54 PM.png)

###DialogPanel Script Component 
The image below shows part of the Inspector panel for the SimpleDialogPanel.  It shows a CanvasGroup component and a DialogController script component have been added. The DialogController script has a DialogList with adjustable size, each element can hold some dialog text, which will be displayed sequentially when the scene is played.

![](/assets/Screen Shot 2018-03-01 at 4.30.08 PM.png)

###Dialog Script Logic - DataStructures: List, Queue
Now, we just have to figure out how to write the code logic. We need an List of strings that can hold our dialog items, then we need to display them sequentially when the next button has been clicked.

Unity can display for editing, both List< string >, or array: string[] in the inspector, as shown in the image above. 


**List< T >** is part of the System.Collections.Generic Namespace in C#.  [MSDN Reference](https://msdn.microsoft.com/en-us/library/6sh2ey19.aspx)


**Queue< T >** is a data structure that operates like a queue / waiting line.  It will make it easy to remove each sequential dialog item from the collection so it can be displayed in sequence.[MSDN Reference](https://msdn.microsoft.com/en-us/library/7977ey2c.aspx)

###DialogController.cs Custom Script
Below is the code where we declare the object reference variables for the dataStructures and components that we need for implementing our logic

- **Declare Object Reference Variables**
    In the code below we specify that the `List< T >` and `Queue< T >` will both be collections of `string` objects.
    Then we declare object reference variables for the components we'll interact with.

```java

public class DialogController : MonoBehaviour {

    [TextArea] //show large textarea in inspector for input
    public List<string> dialogList = new List<string>();

    private Queue<string> queue = new Queue<string>();

    //component object reference variables
    private Button nextBtn;
    private Text dialogText;
    private CanvasGroup cg;

```

- **Initialize Object Reference Variables**

The following code would be located in the Unity Start( ) event function.

```java
        nextBtn = GetComponentInChildren<Button>();   
        nextBtn.onClick.AddListener(GetNextDialog);
        
        cg = GetComponent<CanvasGroup>(); //on this Panel GameObject 
        
        dialogText = GetComponentInChildren<Text>();
        
```
        
- **GetComponentInChildren< T >();**
The Unity method: GetComponentInChildren provides a convenient way to initialize an object reference variable based on the Hierarchy panel's parent-child relationships between gameObjects.  
        
- **Populate the queue data structure** 
The following code is also in the Unity Start() event function.  In the code below, we use a foreach structure, which works like a for-loop, to step through each item in the List< string >: dialogList, and puts the string element into the queue.  Then, we check to make sure there are some items in the queue, we remove the first item, and use it to set the text to be displayed in the dialog panel.
     
     
```java
        //
        foreach(string dialogItem in dialogList){
            queue.Enqueue(dialogItem); //put all of the dialog items into the queue
        }
       
       
        if (queue.Count != 0) //check to make sure some dialog
        {
            dialogText.text = queue.Dequeue(); //display the first dialog item
        }
       
```

###GetNextDialog Method
The nextButton allows the user to move forward through the dialog items.  In the Start( ) method, we configured the nextButton's onClick method to execute the GetNextDialog method each time it is clicked.  The code below shows that each time the GetNextDialog method is executed, it first checks to make sure there are items in the queue.  Then, the dialogText is updated to display the first item in the queue.  The Queue< T > Dequeue( ) method retrieves and removes the item at the front of the queue returns that value so it can be used to set the text value for the dialogText.  If there are no more items in the queue, then the panel is hidden, using the Utility class static method: HideCG.



```java
public void GetNextDialog(){
        if(queue.Count != 0){
            dialogText.text = queue.Dequeue(); //pull out the first item and set to be displayed
        }
        else{  //hide the whole panel
            Utility.HideCG(cg);
            //Dialog over - decision panel?
        }
    }


```



