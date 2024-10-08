## ViewModel

Part of the Android Architecture Components, ViewModels are designed to store and manage UI-related data in a lifecycle-conscious way. 
They survive configuration changes (like screen rotations) and help separate the UI logic from the business logic.
```
class MyViewModel : ViewModel() {
    private val _data = MutableLiveData<String>()
    val data: LiveData<String> get() = _data

    fun updateData(newData: String) {
        _data.value = newData
    }
}
```

Use the ViewModel class to store UI-related data in a lifecycle-conscious way. This allows data to survive configuration changes like screen rotations.
The ViewModel acts as a centralized place to hold the state of your UI, effectively "hoisting" it above the individual UI components.
```
class MyViewModel : ViewModel() {
    var myState: MutableLiveData<String> = MutableLiveData("Initial State")
}
```

## Live Data
A lifecycle-aware observable data holder. LiveData allows UI components to observe changes in data while respecting the lifecycle of those components, 
helping prevent memory leaks and crashes.
```
myViewModel.data.observe(this, Observer { value ->
    // Update UI with new value
})
```
Use LiveData to observe changes in the state. When the data changes, any UI component observing this data will automatically update.
```
class MyActivity : AppCompatActivity() {
    private lateinit var viewModel: MyViewModel

    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        setContentView(R.layout.activity_main)

        viewModel = ViewModelProvider(this).get(MyViewModel::class.java)

        viewModel.myState.observe(this, Observer { newState ->
            // Update your UI based on the new state
        })
    }
}
```

## State Hoisting (mutableStateOf)
A pattern where state is moved up to a higher-level composable or component. 
This allows for better state sharing and management across composables.
```
@Composable
fun MyScreen() {
    var text by remember { mutableStateOf("") }
    TextField(value = text, onValueChange = { text = it })
}
```

## State Management Libs

MVI (Model-View-Intent): A pattern that represents the UI state as a single model, with intents driving state changes. 
Libraries like Mavericks and Redux can be used.

MVVM (Model-View-ViewModel): Separates UI (View) from business logic (ViewModel) and data (Model), making state management clear and maintainable.

## Shared Flow and StateFlow
Part of Kotlin’s Coroutines library, StateFlow is a state-holder observable flow that can be used to manage UI state reactively. 
SharedFlow is useful for events that should be handled once.
```
class MyViewModel : ViewModel() {
    private val _state = MutableStateFlow("Initial State")
    val state: StateFlow<String> get() = _state

    fun updateState(newState: String) {
        _state.value = newState
    }
}
```

## Data Binding (works primarily with XML layout files)
A feature that allows you to bind UI components in layouts to data sources, enabling automatic updates when the data changes.

Data Binding is a framework that allows you to bind UI components in layouts to data sources in application. 
It automates the synchronization between the UI and the underlying data.

## Composition Local
A way to provide state or data across a composable hierarchy without explicitly passing it through parameters. 
Useful for managing themes, configurations, etc.
```
val LocalColor = compositionLocalOf { Color.Unspecified }

```

______

### State Hoisting in Android
state hoisting is a concept borrowed from React, but it can be applied in various ways to manage state effectively in your apps. While Android doesn't have "state hoisting" as a formal term, you can achieve similar outcomes through proper architecture and component design.

1. Passing state down
For Child components (e.g., fragments or custom views) that need to use the state, pass the state down as parameters. This way, the state is managed in one place (the ViewModel), but it's accessible where needed.
```
class MyChildFragment : Fragment() {
    var state: String? = null

    fun updateState(newState: String) {
        state = newState
        // Update UI accordingly
    }
}

```
2. Avoid duplicate
By using a single ViewModel to manage your app's state, you avoid duplicating state across different fragments or activities. This reduces the chance of bugs arising from inconsistent state management.


——————————
###  Internal state to a composable
In Jetpack Compose, you can manage internal state in a composable using the remember and mutableStateOf functions. 
```
import *

@Composable
fun Counter() {
    // Declare a mutable state variable for the count
    var count by remember { mutableStateOf(0) }

    // Layout for the counter
    Column(
        modifier = Modifier
            .fillMaxSize()
            .padding(16.dp),
        horizontalAlignment = Alignment.CenterHorizontally,
        verticalArrangement = Arrangement.Center
    ) {
        Text(text = "Count: $count", style = MaterialTheme.typography.headlineMedium)

        Row {
            Button(onClick = { count++ }) {
                Text("Increment")
            }
            Spacer(modifier = Modifier.width(8.dp))
            Button(onClick = { if (count > 0) count-- }) {
                Text("Decrement")
            }
        }
    }
}
```

### Internal state and State hoisting
Internal state:

Internal state refers to state that is contained and managed within a single composable. This state is local to that composable and is used to manage its own UI logic. The state is typically managed using remember and mutableStateOf.


State hoisting:

State hoisting is a design pattern in which the state is moved up to a parent composable, allowing child composables to receive the state as a parameter. This approach promotes a clear separation of concerns and allows for better state management, particularly when multiple composables need to share or react to the same state.
