# List< T >

Generic Collection classes includes List< T >, where < T > specifies the dataType of items that will be contained in the list.

```C#
using System.Collections.Generic;


//inside some class

List<string> textList;

Start(){
  textList = new List<string>();
  textList.Add("Hello, Do you want to go on an adventure?");
  textList.Add("First, you'll need to get some supplies");
  textList.Add("I hope you have some money, How much do you have?");

   foreach( string s in textList){
    Debug.Log(s);
 }
}

```