PickUp Prefabs



PickUp Class:



```java
using System.Collections;
using System.Collections.Generic;
using UnityEngine;


public enum PickupType
{
    crate, crystal, rock, heart                 //add as needed
}


public class PickUp : MonoBehaviour
{
    public int Value;    //we will replace with property
    public PickupType type; //what is the PickupType of this object
}

```

