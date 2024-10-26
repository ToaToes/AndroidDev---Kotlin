```
class MainActivity : ComponentActivity() {
    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        enableEdgeToEdge()
        setContent {
            RoomwithViewTheme {


            }
        }
    }
}
```

SetContent -> SetContent View from XML

Inside the content block is where to define the composable function -> which is for UI display
- a Composable is a function that is annotated with the ```@Composable``` annotation and is used to define a part of the user interface (UI). 
- Composables are the building blocks of a Compose UI, allowing you to create a declarative and functional approach to UI design.
- MyComposable() will be invoked when the Activity is created.
- If the Activity is paused or destroyed, the Composable's state will be managed accordingly.


### Composable, what used to called _View_ in XML, now is _Composable_ 
1. _**Declarative UI**_(key): Composables allow you to describe the UI in a way that reflects its current state, rather than specifying how to update the UI when the state changes.
2. Reusable: You can create custom Composables to encapsulate UI logic and behavior, making it easy to reuse across different parts of your application.
3. State Management: Composables can easily manage and react to state changes using state holders like remember and mutableStateOf.
4. Hierarchical Structure: Composables can be nested to create complex UIs, with parent Composables managing child Composables.
```
import androidx.compose.material.Text
import androidx.compose.runtime.Composable

@Composable
fun Greeting(name: String) {
    Text(text = "Hello, $name!")
}

```
And put into the setContent block to run

### Relationship Composable with Activity/Fragments
1. Composables are closely integrated with the Android lifecycle. When you set the content of an Activity using setContent, 
the Composables are managed within the context of that Activity's lifecycle.
2. Recomposed: Will update automatically following the Activity going through lifecycle changes - like a device rotation
3. When Activity destroyed, composable will be automatically removed from memory, (clean up) to prevent memory leak



### Composable in Lifecycle

use lifecycle-aware components, like ```remember```, ```rememberSaveable```, and ```LaunchedEffect```, to handle state in a way that is responsive to lifecycle changes:

- remember: Stores values across recompositions but not across configuration changes.
- rememberSaveable: Stores values across recompositions and configuration changes, allowing you to save state when the Activity is recreated.
- LaunchedEffect: Runs a coroutine that is tied to the lifecycle of the Composable and can restart based on specified keys.

### Composable in Lifecycle and Recomposition 
1. onRestart():

This method is called when the Activity is coming back to the foreground after being stopped.
At this point, the Activity is still not fully resumed, and Composables have not yet been recomposed.
2. onStart():

After onRestart(), onStart() is called. The Activity is now visible to the user but not yet in the foreground.
Still, recomposition does not happen during this stage.
3. onResume():

Finally, when the Activity calls onResume(), it is fully in the foreground and active.
At this point, if any state or data changes have occurred that affect the UI, Composables will be recomposed as needed.

Key Points
Recomposition Trigger: Composables are recomposed based on changes in state or data, not simply because the Activity is in onRestart(). 
When the Activity resumes, if the state affecting the Composables has changed, recomposition will occur.

**Lifecycle Awareness**: You can use remember, rememberSaveable, or other state management techniques to preserve UI state across lifecycle events, 
ensuring a seamless experience when transitioning between states.
