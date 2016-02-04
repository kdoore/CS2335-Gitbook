# Zombie Prefab

In our Zombie project we want to create more interesting or customized gameObjects than we can create using the default shape primitives such as Capsule.  

Primitive ZombieMesh = GameObject.CreatePrimitive(PrimitiveType.Capsule);

We will create Prefabs based on either a composite of primitive shapes, or we can create a prefab based on importing other complex assets.  It is convenient that we can dynamically spawn complex GameObjects entirely from within our custom classes by using PreFab assets.

To create a more complex prefab, we can start with a basic shape primitive like a capsule and then create additional components like a head, arms, legs...where these new gameObjects are made child objects of one parent gameObject.

###Animation: Create Material and Complex PreFab
The animation below shows how to create a new material asset, then this material is placed on a capsule, the capsule is duplicated and scaled to form a head.  Then the head is set as a child of the main capsule in the hierarchy, and finally this compound object is dragged into the Resources folder that we created.  Once in this specially named folder, we can access this prefab by it's name to dynamically span within our custom code.

![](prefabAnimation.gif)

##
