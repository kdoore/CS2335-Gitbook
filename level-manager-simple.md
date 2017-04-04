# Level Manager - Simple

Within the MiniGame, we'd like to have several levels of play. Level is an abstract concept, in this gaming context, what do we mean by several levels of play?  We'd like to break the game-experience into meaningful chunks, this will let the player know they have attempted to complete a task, and the game should provide some feedback to indicate their level of success in this challenge.

In this course, we are focused on the functional and structural design of the layer-change event logic.  The dynamic logic can be structured using a finite state machine, since the level logic is event driven and since the game-system can only be in a single state at any time.   One question we might ask is how can we integrate feedback to enhance player learning using an FSM system model?

### Level Enums

For a mini-game that uses a single scene to implement a series of game-levels, we can use a lightweight Finite State Machine - where we define an enum: LevelState to define a set of states that the mini-game system can be in.  We'll put this code in the LevelManager.cs script file.

```java
public enum LevelState
{
    Start,
    Level1,
    Level2,
    Level3,
    Win,
    End
}
/// inside LevelManager Class

public LevelState curLevel;  //declare a LevelState variable to store the active state
```

### Start\( \)

We can use the Start state to allow for a Splash screen and start-button event to trigger the start of the miniGame, or we can start the game-play use the LevelManager Class's Start\( \) method.  The transtion between levels is managed by the NextLevel\(\) function which is triggered when the PlayerDataUpdate event is broadcast from the GameData component object;

```java
/// Inside LevelManager.cs class script

void Start ()
    {
         curLevel = LevelState.Start;  ///initialize curLevel
         crystalSpawner = GameObject.Find ("CrystalSpawner").GetComponent<CrystalSpawner> ();

         nextLevel ();  //starts the game -
    }
```

### CheckLevelEnd\( \)

We can create a custom function that gets executed to determine if the next level should be loaded.  This check can be performed each time the Player collides with a pickup that changes the score.  This will require that the PlayerController script   
has a reference to the LevelManager so it can call this method when the score is changed.  This may cause problems if we want to use the playerController in a different scene, where the LevelManager does not exist. levelScore is an instance variable that keeps track of the points scored within a level, it gets reset to 0 when the level changes.

```java
public void CheckLevelEnd ( int score )  // where can this get called from?  
    {
        levelScore += score;

        Debug.Log ("Check if level is over" + levelScore);
        ////change the total score display

        if (levelScore > 10) { ///level has changed
            ///reset level value display
            levelScore = 0;   //reset level score

            nextLevel ();  //go to next level
        }
    }
```

### Finite State Machine Logic - nextLevel\( \)

The nextLevel\(\) function provides the FSM logic for the Level management.

```C\#
void nextLevel ()
    {
        switch (curLevel) {
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
             //add code here
            break;
        case LevelState.Level3:
            curLevel = LevelState.Win;
             //add code here
            break;
        case LevelState.Win:
            curLevel = LevelState.End;
            // add code here
            break;
        case LevelState.End:
            //add code here
            break;
        } // end switch

    } // end function
```

# Level-Change Behaviors

The nextLevel\(\) method provides the FSM structure to manage state-change logic, so now we just need to decide what to change in the game-scene when this level-change event occurs.  
One simple change to implement is toggling visibility of different background sprites. We can have logic to change the background sprite when the level changes.

