# ({super.key})

```dart
// Initializing the key parameter in the constructor of the parent class (StatelessWidget or StatefulWidget).
// Long
HomeScreen({Key? key}) : super(key: key);
// Short
const HomeScreen({super.key}); 
// super.key: Refers to the key parameter defined in the parent class (StatelessWidget or StatefulWidget). 
// The Key is a special parameter used by Flutter's widget tree to identify widgets uniquely and optimize updates.
```
Use const for widgets that are immutable and don’t depend on runtime data.

Always pass the key using super.key when defining custom widgets to support efficient widget updates in Flutter's widget tree.

# Stateless Widget

Stateless widgets are used when the UI is not changing dynamically, after the first build. This means the content of the UI is static and immutable.

To create a StatelessWidget, you can type `stless` and press enter. To create a Stateless Widget manually, you need to extend the class definition from StatelessWidget and you also need to override the build method that will return one or more widgets. The following is an example of StatelessWidget:

```dart
class MyStatelessWidget extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return MaterialApp(
        home: Scaffold(
            appBar: AppBar(),
            body: Container(),
        ),
    );
  }
}
```

Even if there is a change in any of the variables associated with the Widget, since the build method will not be triggered again, nothing will be updated on the UI of the Stateless Widget.

# Stateful Widget

To change the state of the Widgets based on the state of the variables associated with the Widgets, we need to use StatefulWidgets. StatefulWidgets are used when the part of the UI changes dynamically after the UI is build for the first time.

To create a Stateful Widget, you need to extend the class definition from StatefulWidget and instead of overriding the build method, you need to override the createState() method. The createState() method returns a State object. Then we create another class that is extended from State and here in this class, we need to override the build method and this build method will return one or more widgets.

To create a StatefulWidget code snippet, you can type `stful` and press enter. The following is an example of a StatefulWidget:

```dart
class MyStatefullWidget extends StatefulWidget {
  const MyStatefullWidget({ super.key });
  @override
  State<MyStatefullWidget> createState() => _MyStatefullWidgetState();
}

class _MyStatefullWidgetState extends State<MyStatefullWidget> {
  @override
  void initState() {
    super.initState();
    // You can use WidgetsBinding to delay the execution  until the widget is fully initialized
    WidgetsBinding.instance.addPostFrameCallback((_) {
        // ...
      });
    });
  }
  @override
  Widget build(BuildContext context) {
    return Container();
  }
}
```

Since the build method is overriden in ClassState, it will be called whenever there is a change in the variables associated with the Widgets present in it and the whole widget will be redrawn once again. In order to notify the state change event, we need to call `setState()` method that will trigger the build method.

# Constructor Functions

Accepting Positional & Named arguments
Initialization
File names should always be lower case and seperated by underscore.
you can use const modifier for imutable widgets for performance improvements

```dart
// Positional and Named arguments
import 'package:flutter/material.dart';
class StyledText extends StatelessWidget {
  // const StyledText(this.text, {super.key}); // text is send as positional arguments. positional arguments are mandatory
  const StyledText({super.key, required this.text}); // text is sent as named arguments. required is added since named arguments are optional but text is required here
  final String text;
  @override
  Widget build(BuildContext context) {
    return Text(text);
  }
}
```

```dart
// Multiple Constructors
import 'package:flutter/material.dart';
var startAlignment = Alignment.topCenter;
var endAlignment = Alignment.bottomCenter;
class GradientContainer extends StatelessWidget {
  const GradientContainer(this.color1, this.color2, {super.key});
  const GradientContainer.purple({super.key})
      : color1 = Colors.deepPurple,
        color2 = Colors.indigo;
  final Color color1;
  final Color color2;
  @override
  Widget build(BuildContext context) {
    return Container(
      decoration: BoxDecoration(
        gradient: LinearGradient(
          colors: [color1, color2],
          begin: startAlignment,
          end: endAlignment,
        ),
      ),
      child: Center(
        child: Text(
          "Hello World",
          style: TextStyle(
            color: Colors.white,
            fontSize: 28,
          ),
        ),
      ),
    );
  }
}
// To use the named constructor:
// body: GradientContainer.purple(),
```

# App Skeleton Samples

```dart
// Sample 1
import 'package:flutter/material.dart';
void main() => runApp(MyApp());

// Sample 2
import 'package:flutter/material.dart';
void main() {
  runApp(MaterialApp(
    home: Scaffold(
        body: Container (
            decoration: BoxDecoration(),
            child: Center()
        ),
    ),
  ));
}

// Sample 3
import 'package:flutter/material.dart';
void main() {
  runApp(MaterialApp(
    home: Scaffold(
      body: StartScreen(),
    ),
  ));
}
class StartScreen extends StatelessWidget {
    const StartScreen({Super.key});

    @override
    Widget build(BuildContext context) {
        return const Container();
    }
}
```

# fff 

































































# CircularProgressIndicator

```dart
// You can use a CircularProgressIndicator to show a loading indicator while an image is being fetched using an ImageProvider. The most common way to do this is by using the Image widget with a loadingBuilder that displays a CircularProgressIndicator until the image is fully loaded.
import 'package:flutter/material.dart';

class ImageWithProgressIndicator extends StatelessWidget {
  final String imageUrl;

  ImageWithProgressIndicator({required this.imageUrl});

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(title: Text('Image with Progress Indicator')),
      body: Center(
        child: Image.network(
          imageUrl,
          loadingBuilder: (BuildContext context, Widget child, ImageChunkEvent? loadingProgress) {
            if (loadingProgress == null) {
              return child; // When the image is fully loaded
            } else {
              return Center(
                child: CircularProgressIndicator(
                  value: loadingProgress.expectedTotalBytes != null
                      ? loadingProgress.cumulativeBytesLoaded / (loadingProgress.expectedTotalBytes ?? 1)
                      : null,
                ),
              );
            }
          },
        ),
      ),
    );
  }
}

void main() {
  runApp(MaterialApp(
    home: ImageWithProgressIndicator(imageUrl: 'https://www.example.com/your-image.jpg'),
  ));
}

```



























# Assets

```yaml
# Include all the files in the assets folder as project assets.
flutter:
  assets:
    - assets/

# Explicitly add files in the assets folder as project assets.
flutter:
  assets:
    - assets/images/image1.png
    - assets/images/image2.png
    - assets/fonts/custom_font.ttf
    - assets/other_files/file1.txt
    - assets/other_files/file2.json
```

After making changes to the pubspec.yaml file, run flutter pub get to ensure the assets are bundled properly. You might need to perform a full restart (flutter run) for the changes to take effect.

```dart
// Use an asset image:
Image.assets('assets/image/dice-2.png'),
```

# TextButton

```dart
      child: TextButton(
        onPressed: () {
          // ...
        },
        child: Text("Roll Dice"),
      ),
```

# Stateful Widget

```dart
import 'package:flutter/material.dart';

class DiceRoller extends StatefulWidget {
  const DiceRoller({super.key});

  @override
  State<DiceRoller> createState() {
    return _DiceRollerState();
  }
}

// Private class: Name start with _:
class _DiceRollerState extends State<DiceRoller> {
  void doStg() {
    setState(() {
      // ...
    });
  }
  @override
  Widget build(BuildContext context) {
    return Text("Hello");
  }
}
```