C# Properties:

[MSDN C# Programming Guide](https://msdn.microsoft.com/en-us/library/w86s7x04.aspx)

Properties are a feature of C# that are designed to support Encapsulation.  

"A property is a member that provides a flexible mechanism to read, write, or compute the value of a private field. Properties can be used as if they are public data members, but they are actually special methods called accessors. This enables data to be accessed easily and still helps promote the safety and flexibility of methods." [MSDN](https://msdn.microsoft.com/en-us/library/x9fsa0sw.aspx) 

Properties are public fields that function to provide a public interface for controlled access to the hidden fields value.  

In the example below, we have a private variable: name.  A public Property: Name is used for accessing the private variable, through accessor methods: get and set.  

```java

//private field variable
private string name;

//public property provides access to modify private field: name.
public string Name{  //property
		get{
			return name;
		}
		set{
			name = value;
		}
	}
	
//example usage - object from class that implements property

string objName = MyObject.Name; //getting the name value
MyObject.Name = "Same";     //setting the value of name
	
	```
	
###Properties Hide Class Implementation Details
