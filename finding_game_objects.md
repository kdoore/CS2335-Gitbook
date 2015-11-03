# Finding Game Objects


###Learning Goals:
- Learn how to access current and interact with scene GameObjects and Components within custom class scripts.
- Learn how to use image files as Unity 2D sprites and UI elements.
- Learn how to create UI-Buttons from images, and to use those button to toggle visibility of UI-Panels.

###Object References: 
When we create object references, we're creating a variable that stores a memory address for an object.  For primitive data-types, variables are a labeled space in memory that stores data of the type that we specified when we defined the variable.  However, objects are more complex data types, the system can't know in advance how large of a space in memory we might need for our object, so instead the variable we create for an object holds a 'pointer' or address of the place in memory where our object will be stored.  This is very nice because it allows us to share the address, we can use it as in input variable for a function, then within the function, changes made actually do change the object itself, since we are referring directly to the object via it's address.  This is different than when we pass primitive types into a function, in that case, only a copy of the variable in actually passed in, this is part of the reason that variable scope is important for primitive data types.  Variable scope behaves somewhat  differently for object references, since we're passing the address

