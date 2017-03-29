#Change Sprites

This code can be attached to a gameObject that has a spriteRenderer, it will allow creation of a list of sprites that can be changed by calling the swapSprite( ) method.

```java

using System.Collections;
using System.Collections.Generic;
using UnityEngine;

public class ChangeSprite : MonoBehaviour
{

	public List<Sprite> sprites;
	private int current;
	private SpriteRenderer renderer;
	// Use this for initialization
	void Start ()
	{
		current = 0;
		renderer = GetComponent<SpriteRenderer> ();
	}

	public void swapSprite ()
	{
		current++;
		if (current < sprites.Count) {
			renderer.sprite = sprites [current];
			Debug.Log ("Sprite changed to index " + current);
		}
	}
}

```
