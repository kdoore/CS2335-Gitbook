#PickUp with Enums


```java

using System.Collections;
using System.Collections.Generic;
using UnityEngine;

public enum PickupType { coin, heart, crystal, rock, gem }

public class PickUp : MonoBehaviour {

    //Unity manages creation of components so no constructors
    public int value;  //differnt for every game object instance - set in inspector
    public PickupType type;

    public void AddItem( )
    {
        GameData.instanceRef.Add(this);
    }
}

```