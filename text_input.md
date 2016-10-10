# Text Input


###Text Input
How to script an input Box:  This code is in one of the State.cs files where we are trying to capture the input from an InputText GameObject on the corresponding Scene.  The difficult part of this code is determining how to capture the event when the user has submitted the input by hitting the enter key.  We need to determine how we can add an event Listener within our State.cs instance.

```
	   
	  // State.cs instance variable
      private InputField playerName;
      
      
      public void InitializeObjectRefs (){
        playerName = GameObject.Find ("PlayerName").GetComponent<InputField> ();
		var se=new InputField.SubmitEvent();   //dynamic variable type //let unity figure out the datatype
		se.AddListener(ProcessInput);
		playerName.onEndEdit=se;
      }
```	


this sets up a call to this created function, the input is a string :  This assumes that in ``StateManager`` we have declared a string public field or property: ``public String playerName``
```
    public void ProcessInput(string arg0){
		Debug.Log (arg0);
		managerRef.playerName=arg0;
		Debug.Log ("Player Name " + managerRef.playerName);
	}
    ```


