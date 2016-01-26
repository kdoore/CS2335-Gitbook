# Switch-Case

[MSDN Reference](https://msdn.microsoft.com/en-us/library/06tc147t.aspx)

The switch statement is a control structure that is similar to using a set of if, else-if blocks, where the control of logic is controlled by a test condition that is compared to one or more case labels.  

Details:  In many languages, the values that can be used for the test condition must be integral values such as int, char, or enum.  C# also allows the use of strings.  The test condition is compared against a set of case values that each have a constant value.  

One benefit for switch statements when compared to nested if, else-if blocks is that switch statements are have a simple logical structure that is easy to understand and they are easy to extend by adding new case sections.

Example: [MSDN Reference](https://msdn.microsoft.com/en-us/library/06tc147t.aspx):
```
int caseSwitch = 1;
switch (caseSwitch)
{
    case 1:
        Console.WriteLine("Case 1");
        break;
    case 2:
    case 3:
        Console.WriteLine("Case 2 and 3");
        break;
    default:
        Console.WriteLine("Default case");
        break;
} // end of switch
```

In C#, we must add break statements to insure that the code does not fall through all statements to hit the end of the switch statement.  We can use default: for debugging.  When 