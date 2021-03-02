# Android Architecture Components
Android architecture components are a collection of libraries that help you design robust, testable, and maintainable apps. Start with classes for managing your UI component lifecycle and handling data persistence.

* Learn the basics of putting together a robust app with the [Guide to app architecture](https://developer.android.com/jetpack/guide).
* Manage your app's lifecycle. New [lifecycle-aware components]() help you manage your activity and fragment lifecycles. Survive configuration changes, avoid memory leaks and easily load data into your UI.
* Use [LiveData]() to build data objects that notify views when the underlying database changes.
* [ViewModel]() stores UI-related data that isn't destroyed on app rotations.
* [Room]() is a SQLite object mapping library. Use it to avoid boilerplate code and easily convert SQLite table data to Java objects. Room provides compile time checks of SQLite statements and can return RxJava, Flowable and LiveData observables.
