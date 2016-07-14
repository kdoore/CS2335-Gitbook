#Custom Event Messaging
In our game application, we have several situations where it will be beneficial to define events for an object.  We have already worked with the UI-Button object's Click() method which raises the OnClick event.  In these cases, we defined an Event-handler method that we wanted to be executed when the OnClick event occured.  In order to associate our Event-handler method with the Button's onClick event, we had to use the Button.onClick.AddListener( ) method.  The AddListener( ) method has a delegate as the input parameter, where the delegate defines the format of a function/method that matches a specific function/ method signature.

##GameData Custom Events:
Since our GameData object instance is persisted across all scenes, then we need to be careful not to create code dependencies between GameData and objects that are only in one specific scene.  The GameData object needs to communicate with other gameObjects, but the best way to have communication to reduce tight coupling between objects is to use custom events.  GameData will be an event publisher, objects within a scene can subscribe to GameData events, to recieve notification (and event data) when events have occurred.  

onPlayerdataUpdate , onMiniGameEnd



###Use custom events to implement logic for the end of the MiniGame:



So, the thing to think about is: what is the event that causes the mini-game to end?   I would guess that the player’s score has changed, or I can assume that there was some player collision event (for a positive or destructive object).  If that’s the case, then when that event happens we need to be able to check whether player-data has reached some threshold value which will cause the level to end, for example, maybe the player has died based on a collision with a destructive force.   

So, for previous Scene/State changes, we know that it’s easy to execute: Application.LoadLevel(nextScene)….from anywhere in our code, but we said that we want to keep the logic for changing scenes in an organized structure - we want to have that change-scene logic associated with the StateManager object instance and probably with the current activeState: MiniGameState object instance.  

The problem with the current activeState is that we don’t know how to execute a method on an instance of that object since we can’t easily access that object as a component of a gameObject.  

So we have a few options - one option is to create a new method that is part of the StateManager class - because we can easily have a reference to the StateManager object instance, because it’s on the gameObject: gameManager - and it also has a public static instance that we can access independently of the gameObjects.  

### Have MiniGameState Subscribe to GameData Event
But a better option may be to have the MiniGameState subscribe to a custom event from GameData - that will notify the MiniGameState when it is time to switch scene and state

The missing piece is that we would like to have more than 1 possibility of what is the next state we go to - so how do we determine what next state we go to?  

We can assume that all information that we need, to determine which is the next state, is stored in GameData….and that we can get that data as part of the playerDataEventArgs.   - We'll assume that the MiniGameState object can use gameData information to logically infer what is the next state/scene to go to) 

We can have the MiniGameState subscribe to a custom event from GameData  ( since that’s where we’re storing all of our game information ) ….we know that we’d like to receive the playerDataEventArgs data package with the event….so let’s create a custom event that only gets fired when we know it’s time to leave the mini-game.- let’s call it:  onMiniGameEnd, it is defined below


###Custom event in GameData: 
```
public event EventHandler<PlayerDataEventArgs> onMiniGameEnd;  
```
Then we need a method in gameData, that can be executed when LevelManager has determined the MiniGame has ended - 

Call this method when the miniGame is over, then onMiniGameEnd event will be broadcast to send notification to subscribing objects.

```
public void miniGameIsOver( ){
    
        PlayerDataEventArgs playerData = new PlayerDataEventArgs();  // call constructor to create empty obj. instance
        GetPlayerDataEventArgs (playerData);  // populate with current playerData
        
        if (onMiniGameEnd != null) {  // some object instance has subscribed for notification
            onMiniGameEnd (this, playerData);  // execute event - send updated playerData
        }
    }
```

###MiniGameState Code - Subscribe to Event

In MiniGameState we need to do 2 things:  
    1. get a reference to GameData, subscribe to the onMiniGameEnd event  - using a method: ChangeScene that matches the event’s (delegate) signature    
    2.  Write a method that we want executed when the onMiniGameEvent occurs

```

public void InitializeObjectRefs (){
        gameData = GameData.instanceRef;
        gameData.onMiniGameEnd += ChangeScene;  // our event-handler code - added as a listener / subscriber to the event
            }

///method to be executed when event notification is recieved.
public void ChangeScene( object sender, PlayerDataEventArgs e){
        if (e.totalScore > 30) {  // go to winning condition state
             managerRef.SwitchState (new WinState ());
            Application.LoadLevel ("winScene");
        } else {  // go to losing condition state
            managerRef.SwitchState (new LoseState ());
            Application.LoadLevel ("LoseScene");

        }
    }
```
###LevelManager code to Trigger GameData: MiniGameOver( )

This cascading trail of code is initially triggered from within LevelManager - this is where the chain of events starts for changing state/scene to end the MiniGame
```
void nextLevel(){
        switch(curLevel){
        case LevelState.Start:
            Debug.Log ("State changed - goto :LoadLevel 1");
            curLevel = LevelState.Level1;
            LoadLevel1 ();
            break;
        case LevelState.Level1:
            curLevel = LevelState.Level2;
            LoadLevel2 ();
            break;
        case LevelState.Level2:
            curLevel = LevelState.Level3;
            LoadLevel3 ();
            break;
        case LevelState.Level3:  //when we're in Level3 and nextLevel() gets called, then we know the minigame is over
            gameData.miniGameIsOver();  //this gameData method will trigger the event to change scenes 
            break;
        }
    }
```



