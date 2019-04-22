#Screen Fader:
Attach script to main camera in a scene to have screen fade-in, fade-out.
Requires a UI-Image gameObject where the default image works, but the color must be set to black.
Used in LevelManager when reloading scene, loading level.



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


    public void StartScene()
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

    public IEnumerator FadeRoutine( )
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
                // ... fade to clear in update
                sceneStarting = true;
                yield break;
            }
            else
            {
                yield return null;
            }
        } while (true);
    }

    //fades to black, fades to clear when changing levels
   public void FadeReset()
    {
        sceneStarting = false;
        StartCoroutine("FadeRoutine");
    }

    //end current scene, parameter is next scene to load
    public void EndScene(int SceneNumber)
    {
        sceneStarting = false;
        StartCoroutine("EndSceneRoutine", SceneNumber);
    }
}//end class
```

###Example Usage
This fade behavior can be called from PlayerController Script when the player has collided with an object that results in death. The image below shows the FadeImage UI-Image that's used to fade the scene, it should be the bottom item in the Canvas hierarchy so it hides all GameObjects. In the top image, we can see it's been added to populate the Fade Img box of the ScreenFader script component that's been added to the Main Camera. The second image shows that it's been dragged off screen. When the script is actively fading, then the image is enabled and resized to cover the Canvas, then its' alpha value is gradually changed.  


```
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
ReloadScene();
//Invoke(ReloadScene, 2f); //for more delay
}
}

public void ReloadScene(){
ScreenFader fader= FindObjectOfType<ScreenFader>();
fader.EndScene((int)GameScene.MiniGame);
}
```
![](/assets/Screen Shot 2018-03-28 at 2.45.27 PM.png)

![](/assets/Screen Shot 2018-03-28 at 2.43.09 PM.png)

