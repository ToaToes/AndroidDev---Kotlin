## Activities

Represents a single screen in an app. It serves as the entry point for user interactions.

## Fragments

A modular section of an activity. Fragments can be reused across different activities and help manage complex UIs.

_________
Fragments in Android (vs Activity)
Link: https://youtube.com/watch?v=-vAI7RSPxOA&ab_channel=PhilippLackner Link: https://www.youtube.com/watch?v=h-NcxT697Nk&ab_channel=Foxandroid

Fragments are modular sections of an (part of) Android activity that allow for more flexible UI design and navigation. They represent a portion of the user interface and can be thought of as sub-activities that have their own lifecycle and can be combined in various ways within an activity. Modularity:

Fragments enable you to build reusable UI components that can be easily shared across different activities. Lifecycle Management:

Fragments have their own lifecycle that is closely tied to the lifecycle of their hosting activity. They go through states like Created, Started, and Resumed. Dynamic UI:

You can add, remove, or replace fragments at runtime, allowing for a flexible UI that can adapt to different screen sizes and orientations. Back Stack Support:

Fragments can be added to the back stack, enabling users to navigate back through fragment transactions like they would with activities. Communication:

Fragments can communicate with their hosting activity and with each other, typically through interfaces or shared ViewModels.

________

## Views

Basic UI elements such as:
TextView: Displays text to the user.
EditText: Allows user input.
Button: Triggers an action when clicked.
ImageView: Displays images.

## Layouts

Containers that define the arrangement of views within an Activity or Fragment. Common layout types include:
LinearLayout: Arranges child views in a single row or column.
RelativeLayout: Positions child views relative to each other.
ConstraintLayout: Provides flexible positioning and resizing of views.
ScrollView: Allows for scrolling content when the layout exceeds the screen size.

## RecyclerView

A flexible view for displaying a large dataset in a list format. It is more efficient than a ListView and supports complex layouts and item animations.

## ViewGroups

Special types of views that can contain other views (children). Examples include FrameLayout, LinearLayout, and RelativeLayout.

## Dialogs

Temporary UI components that provide alerts, prompts, or options. Common types include:
AlertDialog: For simple messages or prompts.
ProgressDialog: To indicate loading or processing.

## Menus

UI elements that provide options and actions to the user, often appearing in the app bar or as context menus.

## Navigation Components

A set of libraries and components that help manage app navigation, including:
NavHostFragment: Hosts the navigation graph.
NavController: Manages app navigation within the NavHost.

## Material Components

A library that implements Material Design components, providing modern UI elements like:
BottomNavigationView
FloatingActionButton
Snackbars

## Data Binding

A feature that allows you to bind UI components in your layouts to data sources in your app using a declarative format.

## ViewModels and LiveData

Part of the Architecture Components, these help manage UI-related data in a lifecycle-conscious way.
