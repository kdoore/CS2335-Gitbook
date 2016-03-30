#Custom Event Messaging
In our game application, we have several situations where it will be beneficial to define events for an object.  We have already worked with the UI-Button object's Click() method which raises the OnClick event.  In these cases, we defined an Event-handler method that we wanted to be executed when the OnClick event occured.  In order to associate our Event-handler method with the Button's onClick event, we had to use the Button.onClick.AddListener( ) method.  The AddListener( ) method has a delegate as the input parameter, so the delegate 

