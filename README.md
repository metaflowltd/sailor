# sailor

![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)
[![pub_package](https://img.shields.io/pub/v/sailor.svg)](https://pub.dev/packages/sailor)

A Flutter package for easy navigation management.

#### Warning: Package is still under development, there might be breaking changes in future.

## Roadmap
- [ ] Core Navigation Features
- [ ] Proper logging when navigating
- [ ] Animations 

## Usage

* Create an instance of `Sailor` and add routes.

```dart
// Routes class is created by you.
class Routes {
  static final sailor = Sailor();

  static void createRoutes() {
    sailor.addRoute(SailorRoute(
        name: "/secondPage",
        builder: (context, args) {
          return SecondPage();
        },
      ));
  }
}
```

* Make sure to create routes before starting the application.

```dart
void main() async {
  Routes.createRoutes();
  runApp(App());
}
```

* Register the routes in `onGenerateRoute` using the `generate` function of `Sailor`.

```dart
class App extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return MaterialApp(
      title: 'Compass Example',
      home: Home(),
      onGenerateRoute: Routes.sailor.generator(),
    );
  }
}
```

* Use the instance of `Sailor` to navigate.

```dart
Routes.sailor.navigate(context, "/secondPage");
```

## Passing Arguments
`Sailor` allows you to pass arguments to the page that you are navigating to.

* Create a class that extends from `BaseArguments`.

```dart
class SecondPageArgs extends BaseArguments {
  final String text;

  SecondPageArgs(this.text);
}
```

* When calling the `navigate` method pass these arguments.

```dart

final response = Routes.sailor.navigate(
  context,
  "/secondPage",
  args: SecondPageArgs('Hey there'),
);
```

* When in the SecondPage, use `Sailor.arguments` to get the passed arguments.

```dart
class SecondPage extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    final args = Sailor.arguments<SecondPageArgs>(context);

    return Scaffold(
      appBar: AppBar(
        title: Text('Compass Example'),
      ),
      body: Center(
        child: Text(args.text),
      ),
    );
  }
}
```

## Log Navigation
Use `SailorLogginObserver` to log the `push`/`pop` navigation inside the application.
Add the `SailorLogginObserver` to the `navigatorObservers` list inside your `MaterialApp`. 

```dart
class App extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return MaterialApp(
      title: 'Compass Example',
      home: Home(),
      onGenerateRoute: Routes.sailor.generator(),
      navigatorObservers: [
        SailorLoggingObserver(),
      ],
    );
  }
}
```

Once added, start navigating in your app and check the logs. You will see something like this.
```
flutter: [Sailor] Route Pushed: (New Route='/', Previous Route='null', Arguments=null)
flutter: [Sailor] Route Pushed: (New Route='/secondPage', Previous Route='/', Arguments=Instance of 'SecondPageArgs')
flutter: [Sailor] Route Popped: (New Route='/secondPage', Previous Route='/', Arguments=Instance of 'SecondPageArgs')
```