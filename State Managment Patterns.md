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

## Live Data
A lifecycle-aware observable data holder. LiveData allows UI components to observe changes in data while respecting the lifecycle of those components, 
helping prevent memory leaks and crashes.
```
myViewModel.data.observe(this, Observer { value ->
    // Update UI with new value
})
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
