# Saving Data - Serialization

MSDN Reference Example: [Binary Serialization](https://msdn.microsoft.com/en-us/library/4abbf6k0%28v=vs.110%29.aspx)

"Why would you want to use serialization? The two most important reasons are to persist the state of an object to a storage medium so an exact copy can be re-created at a later stage, and to send the object by value from one application domain to another. For example, serialization is used to save session state in ASP.NET and to copy objects to the Clipboard in Windows Forms. It is also used by remoting to pass objects by value from one application domain to another."   

In Unity, objects that are marked with the Serialized Attribute, are automatically persisted by the Unity serialization process, however there will be cases when we want more control over serialization of game data.