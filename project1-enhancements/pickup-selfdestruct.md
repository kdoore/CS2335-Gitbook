#Self-Destructive PickUp



```java

[DisallowMultipleComponent] //only one of these scripts allowed on a gameObject
public class PickUp : MonoBehaviour {

    //Unity manages creation of components so no constructors
    public int value;  //different for every game object instance - set in inspector

    private void Start() //this will cause pickups to self-destruct after random range of seconds
    {
        Invoke("DestroyMe", Random.Range(3, 7));
    }

    public void DestroyMe()
    {
        Destroy(gameObject);
    }

}

```

