# NOTICE
## Jetpack Compose doesnot rely on XML Layouts design, so this is just a thought

1. Enable Data Binding
```
android {
    ...
    buildFeatures {
        dataBinding true
    }
}

```

2. Create ViewModel
```
// Create a ViewModel that holds the data you want to display:

import androidx.lifecycle.LiveData
import androidx.lifecycle.MutableLiveData
import androidx.lifecycle.ViewModel

class MyViewModel : ViewModel() {
    private val _userName = MutableLiveData<String>("John Doe")
    val userName: LiveData<String> get() = _userName

    private val _userInput = MutableLiveData<String>()
    val userInput: LiveData<String> get() = _userInput

    fun updateUserName(newName: String) {
        _userName.value = newName
    }
}
```
3. Create XML Layout with Data Binding

Create a layout file (e.g., activity_main.xml) that uses Data Binding:
```
<layout xmlns:android="http://schemas.android.com/apk/res/android">
    <data>
        <variable
            name="viewModel"
            type="com.example.MyViewModel" />
    </data>

    <LinearLayout
        android:layout_width="match_parent"
        android:layout_height="match_parent"
        android:orientation="vertical"
        android:padding="16dp">

        <TextView
            android:text="@{viewModel.userName}"
            android:layout_width="wrap_content"
            android:layout_height="wrap_content"
            android:textSize="24sp" />

        <EditText
            android:text="@={viewModel.userInput}" <!-- Two-way binding -->
            android:layout_width="match_parent"
            android:layout_height="wrap_content"
            android:hint="Enter your name" />

        <Button
            android:onClick="@{() -> viewModel.updateUserName(viewModel.userInput)}"
            android:layout_width="wrap_content"
            android:layout_height="wrap_content"
            android:text="Update Name" />
    </LinearLayout>
</layout>

```
4. Set Up Activity
In your activity (e.g., MainActivity), set up Data Binding and link the ViewModel:
```
import android.os.Bundle
import androidx.activity.viewModels
import androidx.appcompat.app.AppCompatActivity
import androidx.databinding.DataBindingUtil
import com.example.databindingexample.databinding.ActivityMainBinding

class MainActivity : AppCompatActivity() {

    private lateinit var binding: ActivityMainBinding
    private val viewModel: MyViewModel by viewModels()

    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)

        binding = DataBindingUtil.setContentView(this, R.layout.activity_main)
        binding.viewModel = viewModel
        binding.lifecycleOwner = this // Allows LiveData to update the UI automatically
    }
}

```

Explanation
ViewModel: The MyViewModel class holds the user name and input data using LiveData. 
It also provides a method to update the user name based on the input.

Data Binding Layout: The layout file uses Data Binding to connect the UI components with the ViewModel. 
The TextView displays the userName, the EditText allows user input with two-way binding to userInput, 
and the Button calls the updateUserName method when clicked.

Activity Setup: In MainActivity, Data Binding is set up to connect the layout with the ViewModel. 
The lifecycleOwner is set to ensure that LiveData changes automatically update the UI.
