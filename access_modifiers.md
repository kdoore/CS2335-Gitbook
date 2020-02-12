#Access Modifiers

[MSDN C# Programming Guide](https://msdn.microsoft.com/en-us/library/ms173121.aspx)

Access modifiers control access to class instance variables and class methods. 
The default access for class instance variables (fields) and methods in C# is private, if none is specified as part of a declaration / definition then the access is private.  Protected access modifier allows child classes to access a base-class variable as if it were public, but outside the base and child classes, the variable access is restricted as if access were specifiec as private.

C# Access modifiers:
```C#
private //default - variable can only be used within the class definition
public // allows use outside the class definition
protected // can be used in a base class to allow access in child class. Outside base and child classes, the variable is private

```
