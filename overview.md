#Project 1 - Overview

In Project 1, we will create a very simple game so we can become familiar with the Unity Editor and using custom scripts to create GameObjects with custom behaviors.

**Games: **

>A game is a voluntary interactive activity, in which one or more players follow rules that constrain their behavior, enacting an artificial conflict that ends in a quantifiable outcome.  [Eric Zimmerman]

**Game - System:**
>Game: A system that encourages learning through strong feedback mechanisms is a game. [ Daniel Cook](http://www.lostgarden.com/2006/10/what-are-game-mechanics.html)

**Mechanics: **
>Mechanics - a "mechanic", something that connects players' actions with the purpose of the game and its main challenges.   

>Game mechanics are concerned with the actual interaction with the game state, while rules provide the possibility space where that interaction is possible, regulating as well the transition between states. In this sense, rules are modeled after agency, while mechanics are modeled for agency.

>In this object-oriented framework, **rules** could be considered general or particular **properties of the game system and its agents**. **All objects in games have properties**. These **properties are **often either** rules or determined by rules**. These **rules are evaluated by a game loop**, an algorithm that relates the current state of the game and the properties of the objects with a number of conditions that consequently can **modify the game state.** For example, the winning condition, the losing condition and the effects of action in the player's avatar health are calculated when running the game loop. This **algorithm relates rules with mechanics**, exemplifying the applicability of an ontological distinction between rules and mechanics.
[Michael Sicart](http://gamestudies.org/0802/articles/sicart)


###Simple Game Objects
For a simple 2D game, we'll focus on simple 2D game-mechanics and UI-display of game-stats to provide feedback to the player:

**GameObjects:**

1.  A **Player** gameObject that can be **controlled by the user** to **interact** with other objects in the game-world.

    -  User-Input: **Input-Events** control **movement**
    -  Interaction:  **Collision-Events modify: Score, Health,** Collected Items, etc.
    
2. **PickUp objects**: gameObjects that the Player gameObject interacts with to change the game's data.
    - Interaction:  Collision-Events - destroy or inactivate gameObject 
    - Movement - add Rigidbody2D for simple falling movement

3. **GameData** - Singleton gameObject to store score, health, etc.

4. **User Interface - Stats** - Stats Displays - Display changing score, health, inventory to give player feedback about game-play actions.

5. **Game-Manager** - 


**References:**

Zimmerman, E. Narrative, Interactivity, Play, and Games. In Wardrip-Fruin, N. & Harrigan, P. (eds), First
Person, MIT Press, 2004.