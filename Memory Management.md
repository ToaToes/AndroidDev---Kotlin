Link: https://developer.android.com/topic/performance/memory-overview

(Find and Fix MEMORY LEAKS with Leak Canary in Android)
<br/>Link: https://www.youtube.com/watch?v=VvkRe9vP5Oc

**On Device Level**
1. ZRAM: Compressed RAM
ZRAM swap can increase the amount of RAM available in the system by compressing memory pages, and putting
them in a dynamically allocated swap area of memory.
Swap area of memory is compressed, and cant be accessed without uncompressing back into the main memory.

2. Thrashing
For Android in IO process, if things cant be compressed into swap area (swap space running out), processes will be killed automatically,
Unlike Linux, which will have this problem.

When switching to background, apps would save a current state, there might be still memory carries it on when user switch back to using,
or might just start again with the 'state', it might be not seamless. (Like Game been killed in background)

## Memory Leak<br>
1. static view or reference might cause leak -> use ViewModel or SavedInstanceState instead
https://medium.com/@skaran5672011/memory-leakage-and-best-practices-to-avoid-memory-leakage-in-android-a8a268344cf2 

2. Handler but not custom to weak reference -> strong reference will cause memory leak
Create an Activity that uses a Handler to perform a delayed operation, 
### Delayed Toast Message
Let's say you want to display a message 2 seconds after a button is clicked. Here’s how you can implement it using a Handler.

have to use handler.removeCallbacksAndMessages(null) in onDestroy() to clean up reference, <br/>
but in default, handler function use strong reference, so when activity stop, handler still hold to the ```Runnable``` will prevent activity from GC <br/>
so have to custom the handler to weak reference -> **This allows the Activity to be garbage collected even if the Handler still exists.** <br/>

3. Using Listeners -> always unregister when not used in activity
For example: listner used in long time operation, will have to unregister

when we register a listener to Activity or Fragment but forget to unregister it.
These listeners hold strong reference to it and prevent it from being garbage collected even when it is unused.



## Garbage Collection(GC)

In JAVA:
```
String str = new String("Hello, World!");
// No need to free 'str'; GC will handle it.
```
In C / C++:
```
char* str = (char*)malloc(20 * sizeof(char));
// Use 'str'...
free(str); // Must remember to free
```

### 1. Memory Management Basics
Heap Memory: When an application creates objects, they are stored in heap memory. As objects are created and used, memory consumption increases.
Automatic Memory Management: To avoid memory leaks and excessive memory usage, Android uses garbage collection to automatically free up memory from objects that are no longer reachable or needed.

### 2. Garbage Collector
Android Runtime (ART): Since Android 5.0 (Lollipop), the Android Runtime (ART) has replaced the Dalvik VM as the runtime environment. ART uses a more efficient garbage collection mechanism that improves performance.
Generational Garbage Collection: ART employs a generational approach, categorizing objects into different generations based on their lifespan:
Young Generation: New objects are allocated in this space. Most objects are short-lived, so this area is frequently collected.
Old Generation: Objects that survive several garbage collection cycles are moved here. This area is collected less frequently since these objects are expected to have a longer lifespan.

### 3. Garbage Collection Process
Mark and Sweep: The garbage collector typically follows a "mark-and-sweep" algorithm:
Mark Phase: The GC identifies which objects are still in use by traversing the reference graph starting from root references (like static variables, local variables, etc.).
Sweep Phase: It then frees memory occupied by objects that are not marked as reachable.

### 4. Triggering Garbage Collection
Automatic Invocation: Garbage collection runs automatically when the system detects that memory is running low, or it can be triggered manually (though manual triggering is discouraged).
Optimizing Memory Usage: Developers can help the garbage collector by nullifying references to objects that are no longer needed, allowing them to be collected sooner.

### 5. Performance Considerations
GC Pauses: Garbage collection can introduce pauses in application execution, which can affect performance. It’s important for developers to write efficient code to minimize the frequency of GC.
Memory Leaks: Developers should avoid memory leaks, where objects are inadvertently retained in memory longer than necessary. Common causes include static references, inner classes holding references to outer classes, and not unregistering listeners.

### 6. Best Practices
Use Weak References: For objects that should not prevent garbage collection (like listeners or callbacks), consider using weak references.
Profile Memory Usage: Use tools like Android Profiler in Android Studio to monitor memory usage and identify potential memory leaks.
Release Resources: Explicitly release resources (like Bitmaps) when they are no longer needed to aid the garbage collector.

_____

**Example of Activity with GC**

An Activity that holds a reference to a Listener object, and how to properly manage the lifecycle of this listener to prevent memory leaks.
1. Create a listener object
```
class MyListener {
    fun onEvent() {
        // Handle the event
        println("Event triggered")
    }
}
```
2. Create an Activity
```
import android.os.Bundle
import androidx.appcompat.app.AppCompatActivity

class MainActivity : AppCompatActivity() {
    private var listener: MyListener? = null

    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        setContentView(R.layout.activity_main)

        // Initialize the listener
        listener = MyListener()

        // Simulate an event trigger
        listener?.onEvent()
    }

    override fun onDestroy() {
        super.onDestroy()
        // Clear the listener reference to avoid memory leaks
        listener = null
    }
}
/*
Listener Class:
The MyListener class represents an object that can handle events. It's a simple class with an onEvent method.

MainActivity:
Listener Initialization: In the onCreate method, initialize the listener object.
Event Simulation: simulate an event by calling listener?.onEvent().
Memory Management: In the onDestroy method, set the listener reference to null. This action makes the MyListener object eligible for garbage collection when the activity is destroyed, preventing a memory leak.
*/

```
**Avoiding Memory Leaks**
In this example, if we didn't set listener to null in onDestroy, the MyListener object would still hold a reference to the MainActivity. This could prevent the activity from being garbage collected, leading to memory leaks.


_____

**Interesting side thought**
How does java detect that an object needs to be garbage collected:
```
public class GarbageCollectionExample {

    static class MyObject {
        String name;

        MyObject(String name) {
            this.name = name;
        }

        @Override
        protected void finalize() throws Throwable {
            System.out.println(name + " is being garbage collected");
        }
    }

    public static void main(String[] args) {
        // Create a new object
        MyObject obj1 = new MyObject("Object 1");
        MyObject obj2 = new MyObject("Object 2");

        // Reference to obj1 is still held
        System.out.println("Before nullifying obj2");
        
        // Nullify obj2 reference
        obj2 = null;

        // Suggest garbage collection
        System.gc();

        // Sleep for a while to allow GC to run
        try {
            Thread.sleep(1000);
        } catch (InterruptedException e) {
            e.printStackTrace();
        }

        // Now let's nullify obj1 and allow it to be garbage collected as well
        obj1 = null;

        // Suggest garbage collection again
        System.gc();

        // Sleep for a while to allow GC to run
        try {
            Thread.sleep(1000);
        } catch (InterruptedException e) {
            e.printStackTrace();
        }
    }
}
/*
Output:

Before nullifying obj2
Object 2 is being garbage collected
Object 1 is being garbage collected

*/

/*
MyObject Class:
This class has a constructor that takes a name and overrides the finalize() method. The finalize() method is called by the garbage collector before reclaiming the object's memory, and here it prints a message indicating that the object is being garbage collected.

Main Method:
Creating Objects: Two instances of MyObject (obj1 and obj2) are created. At this point, both objects are reachable because they are referenced by obj1 and obj2.
Nullifying Reference: After printing a message, obj2 is set to null. This makes obj2 unreachable since there are no references pointing to it anymore.
Suggesting Garbage Collection: The System.gc() method is called to suggest that the JVM performs garbage collection. Note that this is merely a suggestion; the JVM can choose to ignore it.
Sleeping for GC: The program sleeps for a second to give the garbage collector time to run.
Nullifying Another Reference: obj1 is also set to null, making it unreachable as well.
Another Garbage Collection Suggestion: Again, System.gc() is called to suggest garbage collection.
*/

```

