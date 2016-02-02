# Zombie Prefab

In our Zombie project we want to create more interesting or customized gameObjects than we can create using the default shape primitives such as Capsule.  

Primitive ZombieMesh = GameObject.CreatePrimitive(PrimitiveType.Capsule);

We will create Prefabs based on either a composite of primitive shapes, or we can create a prefab based on importing other complex assets.  It is convenient that we can dynamically spawn complex GameObjects entirely from within our custom classes by using PreFab assets.

To create a more complex prefab, we can add a material 