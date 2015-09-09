#State-Controlled UI GameObjects

For the Number game, we want to create a graphical version of the game where the prompts are displayed on the game screen.  We will work in 2D mode, and we need to use a UI game object to  

{% mermaid %}
graph TD;
  A(Start)-->B(Game);
  B(Game)-->C(Win);
  B(Game)-->D(Lose);
  D(Lose)-->E(End);
  C(Win)-->E(End);
{% endmermaid %}


``` sequence
Title: Here is a title
A(Start)->B: Normal line
B-->C: Dashed line
C->>D: Open arrow
D-->>A: Dashed open arrow
```