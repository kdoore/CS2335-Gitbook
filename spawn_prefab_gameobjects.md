# Spawn Prefab GameObjects
In order to dynamically spawn game objects in a scene, we'll need to first create a Prefab of a gameObject.  A prefab is an asset that we can include in any scene, we can consider that it is a preconfigured gameObject, which can include any valid components such as script and animator as pre-configured components which are instantiated when an instance of a Prefab is instantiated in a scene.  

###Melting Crystal
In our mini-game, we want to spawn animated crystals - which melt.  So we need to create a 2D sprite gameObject, then we'll need to attach an animator component and create an animation clip that reduces the sprite's transform scale.x, scale.y values using keyframes.  We want the crystal to generate a custom event when it's destroyed, so we're using PickUp as a base-class and we've defined an OnDiedHandler delegate and an OnDied event.  

###

