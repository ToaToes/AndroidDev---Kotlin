Link: https://developer.android.com/topic/performance/memory-overview

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


## Garbage Collection(GC)

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

