# Switch-Case

[MSDN Reference](https://msdn.microsoft.com/en-us/library/06tc147t.aspx)

The switch statement is a control structure that is similar to using a set of if, else-if blocks, where the control of logic is controlled by a test condition that is compared to one or more case labels.  

Details:  In many languages, the values that can be used for the test condition must be integral values such as int, char, or enum.  C# also allows the use of strings for case-label values.  The test condition is compared against the case-label values that each have a constant value.  

One benefit of switch statements, when compared to nested if, else-if blocks, is that switch statements have a clear logical structure which is easy to understand and to extend by adding new case sections.

Example:
```java
int testValue = 1;
switch (testValue)
{
    case 1:
        Debug.Log("Case 1");
        break;
    case 2: 
    case 3:  
        Debug.Log("Case 2 and 3");
        break;
    case 4:
        Debug.Log("Case 4");
        break;
    default:
       Debug.Log("Default case");
        break;
} // end of switch
```

In C#, we must add break statements to insure that the code does not fall through all statements to hit the end of the switch statement.  We should always add a default: case for debugging, the default case will execute if none of the other case sections have matched the switch test value then execution will fall through to the default case, we should include debugging output so we're aware that the switch - testValue didn't match any of the case label values.

In the code section above, we can see a match with case 2 label does not have any individual case statements to be executed, instead, the execution will progress to the statements inside case 3 since there is no break statement in case 2 to cause execution to jump out of the entire switch statement.  This structure of having several case labels grouped together that correspond to one set of statements to be executed (the case 3 statements), is very useful when checking character input, where case 'a': and case 'A' both correspond to a single block of code to be executed. 

###Equivalency with If-else Statement
The switch statement code above is equivalent to the following if, else-if structure:

```java
if( testValue == 1){
     Debug.Log("Case 1");
}
else if( testVal == 2 || testVal == 3){
    Debug.Log("Case 2 and 3");
}
else if(testVal ==4){
    Debug.Log("Case 4");
}
else{
    Debug.Log("Default case");
}
     
```

The code above can be changed to use a switch structure because the test for each if-else condition is always testing against a single variable: testValue, and the comparison value is always a constant value, rather than an expression which must first be evaluated.  A switch statement can always be transformed into an if, if-else statement but the reverse is not always true, not every if, else-if statement can be converted to an equivalent switch statement.

