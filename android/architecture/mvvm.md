# Links

## GitHub

- [florina-muntenescu/MVPvsMVVM](https://github.com/florina-muntenescu/MVPvsMVVM)
- [MindorksOpenSource/android-mvvm-architecture](https://github.com/MindorksOpenSource/android-mvvm-architecture)

## Medium

- [MVVM — It’s All In The details](https://medium.com/upday-devs/mvvm-its-all-in-the-implementation-details-40ed24871a02)
- [Android by example : MVVM +Data Binding](https://medium.com/@husayn.hakeem/android-by-example-mvvm-data-binding-introduction-part-1-6a7a5f388bf7)
- [MVVM architecture, ViewModel and LiveData](https://proandroiddev.com/mvvm-architecture-viewmodel-and-livedata-part-1-604f50cda1)
- [Approaching Android with MVVM](https://labs.ribot.co.uk/approaching-android-with-mvvm-8ceec02d5442)


## Tutorial

- [The MVC, MVP, and MVVM Smackdown](https://academy.realm.io/posts/eric-maxwell-mvc-mvp-and-mvvm-on-android/)
- [ANDROID ARCHITECTURE COMPONENTS – LOOKING AT ROOM AND LIVEDATA ](https://riggaroo.co.za/android-architecture-components-looking-room-livedata-part-1/)

## Video

- [MVVM architecture with the data binding library - lecture by P. Löwenstein - Code Europe Spring 2017](https://www.youtube.com/watch?v=BYw7ubEFIV8)
- [Android MVP vs MVVM and the winner is...](https://www.youtube.com/watch?v=ugpC98LcNqA)


# Key Concepts

## Model Responsibilities

In general, model is the simplest one to understand. It is the client side data model that supports the views in the application.

It is composed of objects with properties and some variables to contain data in memory.

Some of those properties may reference other model objects and create the object graph which as a whole is the model objects.

Model objects should raise property change notifications which in WPF means data binding.

The last responsibility is validation which is optional, but you can embed the validation information on the model objects by using the WPF data binding validation features via interfaces like INotifyDataErrorInfo/IDataErrorInfo

## View Responsibilities

The main purpose and responsibilities of views is to define the structure of what the user sees on the screen. The structure can contain static and dynamic parts.

Static parts are the XAML hierarchy that defines the controls and layout of controls that a view is composed of.

Dynamic part is like animations or state changes that are defined as part of the View.

The primary goal of MVVM is that there should be no code behind in the view.

It’s impossible that there is no code behind in view. In view you at least need the constructor and a call to initialize component.

The idea is that the event handling, action and data manipulation logic code shouldn’t be in the code behind in View.

There are also other kinds of code that have to go in the code behind any code that's required to have a reference to UI element is inherently view code.

## ViewModel Responsibilities

ViewModel is the main point of MVVM application. The primary responsibility of the ViewModel is to provide data to the view, so that view can put that data on the screen.

It also allows the user to interact with data and change the data.

The other key responsibility of a ViewModel is to encapsulate the interaction logic for a view, but it does not mean that all of the logic of the application should go into ViewModel.

It should be able to handle the appropriate sequencing of calls to make the right thing happen based on user or any changes on the view.

ViewModel should also manage any navigation logic like deciding when it is time to navigate to a different view.
