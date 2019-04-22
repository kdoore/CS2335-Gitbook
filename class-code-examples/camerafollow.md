#CameraFollow

This script can be attached to the mainCamera to have the camera follow the player if there are scrolling images.  




```java
using UnityEngine;

//https://learning.oreilly.com/library/view/learning-2d-game/9780133523416/ch11lev2sec19.html#ch11lev2sec19

    //this script can be added to the camera in a 2D game so the camera follows the player
    //with smoothed following when the player nears the margins of the camera's view.
public class CameraFollow : MonoBehaviour
{
    public float xMargin = 1;
    public float yMargin = 1;
    public float xSmooth = 4;
    public float ySmooth = 4;
    public Vector2 maxXAndY = 20;
    public Vector2 minXAndY=0;

    private Transform player;

    void Awake()
    {
        player = GameObject.FindGameObjectWithTag("Player").transform;
    }

    bool CheckXMargin()
    {
        return Mathf.Abs(transform.position.x - player.position.x) > xMargin;
    }

    bool CheckYMargin()
    {
        return Mathf.Abs(transform.position.y - player.position.y) > yMargin;
    }

    void FixedUpdate()
    {
        TrackPlayer();
    }

    public void ResetPosition()
    {
        transform.position = new Vector3(0, 0, -10);
    }

    void TrackPlayer()
    {
        float targetX = transform.position.x;
        float targetY = transform.position.y;

        if (CheckXMargin() == true)
        {
            targetX = Mathf.Lerp(transform.position.x,
              player.position.x, xSmooth * Time.deltaTime);
        }
        if (CheckYMargin() == true)
        {
            targetY = Mathf.Lerp(transform.position.y,
              player.position.y, ySmooth * Time.deltaTime);
        }

        targetX = Mathf.Clamp(targetX, minXAndY.x, maxXAndY.x);
        targetY = Mathf.Clamp(targetY, minXAndY.y, maxXAndY.y);

        transform.position = new Vector3(targetX, targetY,
    transform.position.z);
    }
} //end class
```

