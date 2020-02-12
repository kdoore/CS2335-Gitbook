# Change Sprites

This code can be attached to a gameObject that has a spriteRenderer, it will allow creation of a list of sprites that can be changed by calling the swapSprite\( \) method.

```java
using System.Collections;
using System.Collections.Generic;
using UnityEngine;

public class ChangeSprite : MonoBehaviour
{

    public List<Sprite> sprites = new List<Sprite>(); //initialize list before Start( )
    private int current;
    private SpriteRenderer spriteRenderer;
    // Use this for initialization
    void Start ()
    {
        current = 0;

        spriteRenderer = GetComponent<SpriteRenderer> ();
    }

    public void swapSprite ()
    {
        current++;
        if (current < sprites.Count) {
            spriteRenderer.sprite = sprites [current];
            Debug.Log ("Sprite changed to index " + current);
        }
    }
}
```

The image below shows the BackgroundImage GameObject, where the ChangeSprite Script has been added as a script component.  Here we can see that the spriteList in the inspector has been set to hold 4 elements, then when expanded, the 4 slots have been populated with different sprites that can be swapped out for the background image.  

### Use in LevelManager.cs

public void swapSprite\( \)  has been declared as a public method so that it can be called from the LevelManager:  In order to do that, the LevelManager must create an objectReference to the ChangeSprite Script:  



```
//other code above here to declare object references

ChangeSprite changeSprite;

void Start ()
	{
	//other code
	changeSprite = GameObject.Find ("BackgroundImage").GetComponent<ChangeSprite> ();
	}

//when loading Level2, the background sprite gets changed using the code below.	
void loadLevel2 ()
	{
		///change background image
		changeSprite.SwapSprite ();
		spawner.StartSpawning ();
		levelValue.text = "2";
	}	

```

![](/assets/Screenshot 2017-04-04 11.17.55.png)

