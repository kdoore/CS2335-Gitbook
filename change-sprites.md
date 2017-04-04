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



