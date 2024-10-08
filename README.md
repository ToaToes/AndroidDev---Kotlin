# AndroidDev and Compose
Some reminds in AndroidDev process

## Material Design in Android Dev
Key features of Material Design include:

Material Metaphor: This concept uses tactile surfaces and realistic lighting to create a sense of depth. Elements look and behave like physical materials, providing a familiar experience for users.

Bold, Graphic, and Intentional: It emphasizes bold colors, large typography, and intentional white space to create a visually appealing and easy-to-navigate interface.

Motion Provides Meaning: Animations are used to guide users, indicate actions, and provide feedback, enhancing the overall experience rather than distracting from it.

Adaptive Design: Material Design promotes responsive and adaptive layouts that adjust to various screen sizes and orientations, ensuring a consistent experience across devices.

Components and Patterns: It includes a rich library of UI components and design patterns, making it easier for developers to create consistent and functional applications.


## Basic Layout in Android Compose
Column
```
Column {
    Text("Item 1")
    Text("Item 2")
}
```

Row
```
Row {
    Text("Item 1")
    Text("Item 2")
}
```

Box
```
Box {
    Image(painterResource(R.drawable.image), contentDescription = null)
    Text("Overlay Text", modifier = Modifier.align(Alignment.Center))
}
```

Modifier
```
Text("Hello", modifier = Modifier.padding(16.dp))
```

Spacer
```
Row {
    Text("Item 1")
    Spacer(modifier = Modifier.width(16.dp))
    Text("Item 2")
}

```

LazyColumn
```
LazyColumn {
    items(listOf("Item 1", "Item 2", "Item 3")) { item ->
        Text(item)
    }
}
```

Card
```
Card {
    Text("This is a card")
}
```

Scaffold
```
Scaffold(
    topBar = { TopAppBar(title = { Text("Title") }) },
    floatingActionButton = { FloatingActionButton(onClick = {}) { Icon(Icons.Filled.Add, contentDescription = null) } }
) { innerPadding ->
    // Content goes here
}
```
