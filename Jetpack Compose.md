Jetpack Compose is a modern UI toolkit for building native Android interfaces. It simplifies UI development by using a declarative approach, allowing developers to describe their UI components and how they should behave in response to data changes.

**Declarative UI:** _UI on coding not XML_ <br/>
Instead of dealing with XML layouts, you write Kotlin code to define your UI. This makes it easier to visualize and manage UI states.

**Composable Functions:** _Reused_ <br/>
You create UI elements as composable functions, which can be combined and reused. Each function can manage its own state and layout.

**State Management:** _View, state Update, only UI part affect_ <br/>
Compose efficiently handles state updates. When the state changes, only the affected parts of the UI are recomposed, improving performance.

**Integration with Existing Android Code:** _eazy to adapt existing code_
You can integrate Jetpack Compose with existing views and UI components, allowing for a gradual adoption.

**Material Design Support:** _原生 有官方以及社区支持_
Compose comes with built-in support for Material Design, enabling developers to create visually appealing and consistent UIs.

**Tooling:** _支持preview 可提前看UI 状态更新_
It offers excellent tooling support in Android Studio, including a preview feature to see UI changes in real time without needing to run the app.

Overall, Jetpack Compose aims to make Android UI development faster, more intuitive, and more flexible.


## Modifier
```
modifier = Modifier
  backgound(Color.Red)
  padding(16.dp)
  backgroun(Color.Green) -> this will only apply to the padding, not affecting the last background 

AND

modifier = Modifier
  .background(Color.Red)
  .background(Color.Green) -> this will override the last background
  .padding(16.dp)

```

## TextField, OutlinedTextField, BasicTextField
