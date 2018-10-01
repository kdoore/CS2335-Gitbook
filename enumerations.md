## Enum ( Enumeration)
[MSDN reference](https://msdn.microsoft.com/en-us/library/sbbt4032.aspx)

The **enum** keyword is used to declare an enumeration, a distinct type that consists of a set of named constants called the enumerator list.

```public enum Weekdays {monday, tuesday, wednesday, thursday, friday};```

Enumerations are useful when designing state-based programs because we can define meaningful states labels and we can use those names in our program to in our control statements to test and query the  the active state. 

```cpp
public enum GameStates {start, game, win, lose, end};
private GameStates activeState;  //variable to hold active state

void start(){
    activeState = GameStates.start;  //set one state as active
}

	
void update(){
    if(activeState == GameStates.start){
        Debug.Log( activeState.ToString() );  //this allows us to print out the labeled name, otherwise we'd see the integer value of the state: 0;
    }
   }
```
