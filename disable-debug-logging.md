#Disable Debug.Log Console Output for a Script

[Unity Reference ](https://docs.unity3d.com/ScriptReference/Logger-logEnabled.html)
 
 Adding the following 2 lines of code to any script will suppress Debug.Log output to the console.  Include this code in your files, and keep as a comment while designing your game so you'll get your debug info.  Then activate by uncommenting the code: `logger.logEnabled = Debug.isDebugBuild;`
Below the code is active, so the debug message will not show in the console unless running in debug mode.
   

```java
  
    /// allows for debugging to be disabled for a script
    private static ILogger logger = Debug.unityLogger;

	void Start () {
        logger.logEnabled = Debug.isDebugBuild;
        
        Debug.Log("Test Debug Logging");
	}
```


	