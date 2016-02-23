# Foreach

[MSDN Reference](https://msdn.microsoft.com/en-us/library/ttw7t8t6.aspx)

"The foreach statement is used to iterate through the collection to get the information that you want", but should not be used to add or remove items from the source collection. "If you need to add or remove items from the source collection, use a for loop."

Example:  Given a Monster constructor: Monster(int n)

```java
List<Monster> monsterList = new List<Monster>();  //Initialize List<T> using List<T> constructor

monsterList.add(new Monster(5));  //call Monster Constructor
monsterList.add(new Monster(6));
monsterList.add(new Monster(7));

foreach( Monster m in monsterList){
    Debug.Log(m.ToString());
}
```