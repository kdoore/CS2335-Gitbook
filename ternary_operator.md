# Ternary Operator: ?

[MSDN Reference:](https://msdn.microsoft.com/en-us/library/ty67wk28.aspx)

The Ternary Operator: ? , is a conditional operator that provides a shortcut assignment operator based on whether a condition evaluates to true or false. 

The same logic can be represented using if - else logic, but Ternary operator provides a more concise format when using conditional logic to determine the value assigned to a variable.

Example logic: Goal - determine value to be assigned to index  
* if someTest is true
* then set index = 5
* otherwise, index = 10

```C#
int index=0; // initialze value

if( someTest == true){
   index = 5;
}else{
   index = 10;
}
/////  Same Logic using Ternary Operator:

index = (someTest == true) ? 5 : 10;  
```


