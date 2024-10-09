**Contexts**
Link: https://stackoverflow.com/questions/3572463/what-is-context-on-android

**Strong Reference VS. Weak Reference**
A strong reference to an object in an Android activity, that reference will continue to exist even if the activity is destroyed. 
This means the object won't be garbage collected as long as the strong reference remains, which can lead to memory leaks if you're not careful.

For example, if you have a static reference to the activity or any other heavy resources, those will persist even after the activity's lifecycle has ended, 
preventing the associated memory from being released.

To avoid this, consider using weak references when appropriate, 
or properly managing your references in the activity's lifecycle methods (like releasing resources in onDestroy()).

```
import java.lang.ref.WeakReference

class MyClass(val name: String)

fun main() {
    // Create a strong reference to MyClass
    var strongReference: MyClass? = MyClass("My Object")

    // Create a weak reference to MyClass
    val weakReference = WeakReference(strongReference)

    // Check the value of the weak reference
    println("Before clearing strong reference: ${weakReference.get()?.name}") // Outputs: My Object

    // Clear the strong reference
    strongReference = null

    // Now the object can be garbage collected
    System.gc() // Suggest garbage collection

    // Check the value of the weak reference again
    println("After clearing strong reference: ${weakReference.get()?.name}") // May output: null
}

```


**Async Task**
AsyncTask is designed to work with the UI thread and update UI elements. If the activity that started the task is no longer alive (for example, 
if it was finished or destroyed), any attempt to interact with the UI can result in exceptions.

While **coroutines** are often seen as a modern replacement for AsyncTask in Android development. 
They provide a more flexible and powerful way to manage asynchronous operations and simplify code for handling background tasks. 

Couroutines Example:
```
// Inside a ViewModel or Activity
import kotlinx.coroutines.*

fun fetchData() {
    // Launch a coroutine in the Main scope
    viewModelScope.launch {
        try {
            val result = withContext(Dispatchers.IO) {
                // Perform the network request or background task
                fetchDataFromNetwork()
            }
            // Update the UI with the result
            textView.text = result
        } catch (e: Exception) {
            // Handle any exceptions
            Log.e("Error", "Failed to fetch data", e)
        }
    }
}

```

**Companion Object**
Link: https://www.youtube.com/watch?v=qkMwoPrlXHE

_Companion object VS. Multiple Instances_

Companion object:
```
class User private constructor(val name: String, val role: String) {
    
    companion object {
        // Constants for user roles
        const val ROLE_ADMIN = "Admin"
        const val ROLE_USER = "User"
        
        // Factory method to create a User instance
        fun createAdmin(name: String): User {
            return User(name, ROLE_ADMIN)
        }

        fun createUser(name: String): User {
            return User(name, ROLE_USER)
        }
    }
}

// Usage
fun main() {
    val admin = User.createAdmin("Alice")
    val user = User.createUser("Bob")

    println("${admin.name} is a ${admin.role}") // Outputs: Alice is a Admin
    println("${user.name} is a ${user.role}")   // Outputs: Bob is a User
}

```
Instances:
```
class MyClass(val name: String)

val instance1 = MyClass("Alice")
val instance2 = MyClass("Bob")

println(instance1.name)  // Outputs: Alice
println(instance2.name)  // Outputs: Bob

```




