#Level Manager Event-Driven System: 

###Event-Logic Diagram

Game Events trigger execution of LevelManager methods: 
   
    - GameData.onPlayerDataUpdate --> MiniGameOver( )
    - LevelManager: Timer complete --> ReloadMiniGame( ) 
    
    - PlayerController.onPlayerDied --> MiniGameOver( )
    
    - GameData.onPlayerDataUpdate --> NextLevel( ) 
    
    - PlayerController.onPlayerReachExit -->NextLevel( )
    
    - LevelManager: Timer complete --> ReloadMiniGame( ) 
   

NextLevel( ) implements FiniteStateMachine based on variable: LevelState:  curLevel

![](/assets/Screen Shot 2019-04-23 at 10.57.40 AM.png)