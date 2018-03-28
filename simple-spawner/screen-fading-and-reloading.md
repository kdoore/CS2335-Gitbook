###Fade when loading and reloading Scenes

This screen fading script uses Co-routines to call a function repeatedly.  The alpha value of an image is slowly modified over several frames because it's modified only slightly each time.


```java

using UnityEngine;
using UnityEngine.UI;
using System.Collections;
using UnityEngine.SceneManagement;

//Screen Fader Script from NovaSurfer on Github
//https://gist.github.com/NovaSurfer/5f14e9153e7a2a07d7c5
//NovaSurfer on GitHub
//create a UI Image, move it off-screen, give it a black color.
//Attach this script to the main camera in each scene where you want fading
//populate the FadeImage with your UI Image in the inspector

public class ScreenFader : MonoBehaviour
{
    public Image FadeImg;
    public float fadeSpeed = 1.5f;
    public bool sceneStarting = true;

    void Awake()
    {
        FadeImg.rectTransform.localScale = new Vector2(Screen.width, Screen.height);
    }

    void Update()
    {
        // If the scene is starting...
        if (sceneStarting)
            // ... call the StartScene function.
            StartScene();
    }


    void FadeToClear()
    {
        // Lerp the colour of the image between itself and transparent.
        FadeImg.color = Color.Lerp(FadeImg.color, Color.clear, fadeSpeed * Time.deltaTime);
    }


    void FadeToBlack()
    {
        // Lerp the colour of the image between itself and black.
        FadeImg.color = Color.Lerp(FadeImg.color, Color.black, fadeSpeed * Time.deltaTime);
    }


    void StartScene()
    {
        // Fade the texture to clear.
        FadeToClear();

        // If the texture is almost clear...
        if (FadeImg.color.a <= 0.05f)
        {
            // ... set the colour to clear and disable the RawImage.
            FadeImg.color = Color.clear;
            FadeImg.enabled = false;

            // The scene is no longer starting.
            sceneStarting = false;
        }
    }


    public IEnumerator EndSceneRoutine(int SceneNumber)
    {
        // Make sure the RawImage is enabled.
        FadeImg.enabled = true;
        do
        {
            // Start fading towards black.
            FadeToBlack();

            // If the screen is almost black...
            if (FadeImg.color.a >= 0.95f)
            {
                // ... reload the level
                SceneManager.LoadScene(SceneNumber);
                yield break;
            }
            else
            {
                yield return null;
            }
        } while (true);
    }

    //end current scene, parameter is next scene to load
    public void EndScene(int SceneNumber)
    {
        sceneStarting = false;
        StartCoroutine("EndSceneRoutine", SceneNumber);
    }
}   

```

###Example Usage
This fade behavior can be called from PlayerController Script when the player has collided with an object that results in death.

void OnTriggerEnter2D (Collider2D hitObject)
	{
		if (hitObject.CompareTag ("Collectable")) {
			Debug.Log ("Hit Collectable");
			Destroy (hitObject.gameObject);
			
		}
		if (hitObject.CompareTag ("Hazard")) {
            anim.SetInteger("HeroState", (int)heroState.dead);
			
            Debug.Log ("Hit Hazard event");
			Destroy (hitObject.gameObject);
            dead = true;
            invoke(ReloadScene, 2f);
		}
	}

 public void ReloadScene(){
        ScreenFader fader= FindObjectOfType<ScreenFader>();
        fader.EndScene((int)GameScene.MiniGame);
    }


