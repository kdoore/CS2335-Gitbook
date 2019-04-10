#Inventory Classes:  Item, ItemInstance

The backbone of this Inventory are several classes that inherit from SciptableObject.



##Item Class - Abstract Class, ItemInstance Class
The **Item Class** is an **abstract class**, this means that no Item objects can be created directly of the Item type.  Instead, we must write **concrete classes** that inherit from the Item Class, such as Gem, Potion.  

##Methods:  abstract, virtual, override:
The Item class introduces new types of methods, where we want to insure we can call the method `Use( )`, on all object instances ( of child classes )

**Last Updated: April 10, 2019**
##class Item, ItemInstance
```java

using System.Collections;
using System.Collections.Generic;
using UnityEngine;

//https://github.com/Toqozz/blog/blob/master/inventory

//https://answers.unity.com/questions/1415831/inheritance-from-a-scriptableobject.html

public enum ItemType
{
    Gem, Potion, Key
}

[System.Serializable]
//abstract classes can not be used to make 
//actual object instances, only child classes of 
//abstract classes can actually be created.
public abstract class Item : ScriptableObject {

    public string itemName;
    public int value;
    public Sprite sprite;

    protected ItemType itemType;


    /// <summary>
    /// virtual means that this class can be overridden in a child-class, but it is not required
    /// in which case no code would be executed since this default version of the method 
    /// happens to have no code.
    /// </summary>
    public virtual void Use() 
    {
       //no code in this case, so nothing will be executed
    }

    ///other option which requires Use to be overridden in child classes
    /// 
    /*public abstract void Use()
    {
        //no code in this case, so nothing will be executed
    }
    */
} //end Item class


[System.Serializable]
public class ItemInstance 
{
    // Reference to scriptable object "template".
    public Item item; //should be a child class item


    public ItemInstance(Item item,int value ) 
    {
        this.item = item;
        item.value = value;
    }
}//end ItemInstance class


```

###Class Gem

```java
using System.Collections;
using System.Collections.Generic;
using UnityEngine;

public enum GemType { Ruby, Diamond, Sapphire, Emerald }

[System.Serializable]
public class Gem : Item {

    public GemType gemType;

    public Gem()
    {
        itemType = ItemType.Gem;
    }

    public override void Use()
    {
        Debug.Log("Using Gem");
    }
}

```

##Class Potion



```java
using System.Collections;
using System.Collections.Generic;
using UnityEngine;

public enum PotionType { Energy, Wisdom, Truth, Health }

public class Potion : Item {

    public PotionType potionType;

    public Potion()
    {
        itemType = ItemType.Potion;
    }

    public override void Use()
    {
        Debug.Log("Using Potion " + this.potionType);
    }

}
```



