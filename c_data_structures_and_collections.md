# C# Data Structures and Collections

[MSDN Reference](https://msdn.microsoft.com/en-us/library/7y3x785f.aspx)

C# provides a large variety of special classes that are designed for us to create a collection where we can maintain, access, modify a series of data.  We've learned that Arrays are a basic data structure, a structure for storing data.  

###Arrays
Arrays have a fixed size, so if we declare an Array to hold 100 elements, if our program needs to store 101 elements, then we have a problem.  To solve this, the ArrayList class was created.  The ArrayList size can be changed dynamically, as the program is executing, so we could use an ArrayList to store data as part of some user-input, because we wouldn't need to limit the user's allowed number of input values, but we wouldn't know ahead of time how many elements would be needed.    When we declare an Array, we must specify the type of element that the Array will hold.  `Car[] myCars = new Car[5];` This array will be able to hold object references for Car objects.  