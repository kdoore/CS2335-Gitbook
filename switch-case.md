# Switch-Case

[MSDN Reference](https://msdn.microsoft.com/en-us/library/06tc147t.aspx)

The switch statement is a control structure that is similar to using a set of if, else-if blocks, where the control of logic is controlled by a test condition that is compared to one or more case labels.  

Details:  In many languages, the values that can be used for the test condition must be integral values such as int, char, or enum.  C# also allows the use of strings for case-label values.  The test condition is compared against the case-label values that each have a constant value.  

One benefit of switch statements, when compared to nested if, else-if blocks, is that switch statements have a clear logical structure that is easy to understand and to extend by adding new case sections.

Example:
```
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
    default:
       Debug.Log("Default case");
        break;
} // end of switch
```

In C#, we must add break statements to insure that the code does not fall through all statements to hit the end of the switch statement.  We should always add a default: case for debugging, the default case will execute if none of the other case sections have matched the switch test value then execution will fall through to the default case, we should include debugging output so we're aware that the switch - testValue didn't match any of the case label values.