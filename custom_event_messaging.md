#Custom Event Messaging
In our game application, we have several situations where it will be beneficial to define events for an object.  We have already worked with the UI-Button object's Click() method which raises the OnClick event.  In these cases, we defined an Event-handler method that we wanted to be executed when the OnClick event occured.  In order to associate our Event-handler method with the Button's onClick event, we had to use the Button.onClick.AddListener( ) method.  The AddListener( ) method has a delegate as the input parameter, where the delegate defines the format of a function/method that matches a specific function/ method signature.

#GameData Custom Events:
Since our GameData object instance is persisted across all scenes, then we need to be careful not to create code dependencies between GameData and objects that are only in one specific scene.  The GameData object needs to communicate with other gameObjects, but the best way to have communication to reduce tight coupling between objects is to use custom events.  GameData will be an event publisher, objects within a scene can subscribe to GameData events, to recieve notification (and event data) when events have occurred.  


onPlayerdataUpdate , onMiniGameEnd



