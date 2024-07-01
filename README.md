# MVVM-Android

### MVVM Architecture in a Few Lines

MVVM (Model-View-ViewModel) is an architectural pattern used in software development. 

- **Model**: Represents the data and business logic.
- **View**: The UI layer that displays data to the user.
- **ViewModel**: Acts as a mediator between the Model and the View, handling data presentation logic and user interactions.

This pattern promotes a clear separation of concerns, making the code more manageable, testable, and scalable.

### Structure Files in Android

Here is a simplified structure for an Android project using MVVM:

1. **Model**: 
   - **Data Classes**: Define data models.
   - **Repository**: Handles data operations, typically from a database or network.

2. **View**:
   - **Activities/Fragments**: UI components that display data.
   - **Layouts**: XML files defining the UI.

3. **ViewModel**:
   - **ViewModel Classes**: Manage UI-related data and handle user interactions.

### Example Directory Structure

```
com.example.app
├── data
│   ├── model
│   └── repository
├── ui
│   ├── view
│   └── viewmodel
├── MainActivity.kt
└── res
    └── layout
        └── activity_main.xml
```

- **data/model**: Contains data models.
- **data/repository**: Manages data sources and operations.
- **ui/view**: Holds UI components like Activities and Fragments.
- **ui/viewmodel**: Contains ViewModel classes.

### Explanation of File Placement in MVVM Architecture

1. **Model**
   - **Data Classes**: These classes define the structure of your data. They are usually plain Kotlin or Java objects (POJOs) or data classes.
     - Example: `User.kt`
       ```kotlin
       data class User(val id: Int, val name: String, val email: String)
       ```
     - **Location**: `com.example.app.data.model`

   - **Repository**: This class handles data operations and abstracts the data sources (like local database, network, etc.).
     - Example: `UserRepository.kt`
       ```kotlin
       class UserRepository(private val userDao: UserDao) {
           fun getUser(userId: Int): LiveData<User> {
               return userDao.getUser(userId)
           }
       }
       ```
     - **Location**: `com.example.app.data.repository`

2. **View**
   - **Activities/Fragments**: These are the UI components that interact with the user. Activities and Fragments handle the lifecycle and display data.
     - Example: `MainActivity.kt`
       ```kotlin
       class MainActivity : AppCompatActivity() {
           private lateinit var viewModel: MainViewModel
           
           override fun onCreate(savedInstanceState: Bundle?) {
               super.onCreate(savedInstanceState)
               setContentView(R.layout.activity_main)

               viewModel = ViewModelProvider(this).get(MainViewModel::class.java)

               // Observe ViewModel data and update UI
               viewModel.user.observe(this, Observer { user ->
                   // Update UI
               })
           }
       }
       ```
     - **Location**: `com.example.app.ui.view`

   - **Layouts**: XML files defining the UI components.
     - Example: `activity_main.xml`
       ```xml
       <LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
           android:layout_width="match_parent"
           android:layout_height="match_parent"
           android:orientation="vertical">
           
           <TextView
               android:id="@+id/userName"
               android:layout_width="wrap_content"
               android:layout_height="wrap_content" />
       </LinearLayout>
       ```
     - **Location**: `res/layout`

3. **ViewModel**
   - **ViewModel Classes**: These classes hold and manage UI-related data. They handle user interactions and live data binding.
     - Example: `MainViewModel.kt`
       ```kotlin
       class MainViewModel(application: Application) : AndroidViewModel(application) {
           private val repository: UserRepository = UserRepository(UserDao())
           val user: LiveData<User> = repository.getUser(1)
       }
       ```
     - **Location**: `com.example.app.ui.viewmodel`

### Example Directory Structure

```
com.example.app
├── data
│   ├── model
│   │   └── User.kt
│   └── repository
│       └── UserRepository.kt
├── ui
│   ├── view
│   │   └── MainActivity.kt
│   └── viewmodel
│       └── MainViewModel.kt
├── MainActivity.kt
└── res
    └── layout
        └── activity_main.xml
```

- **com.example.app.data.model**: Contains data classes like `User.kt`.
- **com.example.app.data.repository**: Contains repository classes like `UserRepository.kt`.
- **com.example.app.ui.view**: Contains UI components like `MainActivity.kt`.
- **com.example.app.ui.viewmodel**: Contains ViewModel classes like `MainViewModel.kt`.
- **res/layout**: Contains layout XML files like `activity_main.xml`.
