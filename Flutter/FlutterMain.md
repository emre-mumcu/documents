```dart
// Simple main
import 'package:flutter/material.dart';
void main() {
  runApp(MaterialApp());
}
```
```dart
// Simple main
import 'package:flutter/material.dart';
void main() {
  runApp(MaterialApp(
    home: Text("Welcome"),
  ));
}
```
```dart
// Simple main
import 'package:flutter/material.dart';
void main() {
  runApp(MaterialApp(
    home: Scaffold(
      body: Text("Hello World ;)"),
    ),
  ));
}
```
```dart
// Simple main
import 'package:flutter/material.dart';
void main() {
  runApp(MaterialApp(
    home: Scaffold(
      body: Center(child: Text("Hello World")),
    ),
  ));
}
```
```dart
// Detailed main
import 'package:flutter/material.dart';
void main() {
  runApp(MaterialApp(
    home: Scaffold(
      body: Container(
        decoration: BoxDecoration(
          gradient: LinearGradient(colors: [Colors.black, Colors.white]),
        ),
        child: Center(
          child: Text("Hello World"),
        ),
      ),
    ),
  ));
}
```
```dart
// Detailed main
import 'package:flutter/material.dart';
void main() {
  runApp(MaterialApp(
    home: Scaffold(
      body: Container(
        decoration: BoxDecoration(
          gradient: LinearGradient(
            colors: [Colors.black, Colors.white],
            begin: Alignment.topCenter,
            end: Alignment.bottomCenter,
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
      ),
    ),
  ));
}
```
```dart
// Reusable Widgets
import 'package:flutter/material.dart';
void main() {
  runApp(MaterialApp(
    home: Scaffold(
      body: GradientContainer(),
    ),
  ));
}
class GradientContainer extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return Container(
      decoration: BoxDecoration(
        gradient: LinearGradient(
          colors: [Colors.black, Colors.white],
          begin: Alignment.topCenter,
          end: Alignment.bottomCenter,
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
```
```dart
// Reusable Widgets with different files
// main.dart
import 'package:flutter/material.dart';
import 'package:first_app/gradient_container.dart';
void main() {
  runApp(MaterialApp(
    home: Scaffold(
      body: GradientContainer(),
    ),
  ));
}
// gradient_container.dart
import 'package:flutter/material.dart';
class GradientContainer extends StatelessWidget {
  const GradientContainer({super.key});
  @override
  Widget build(BuildContext context) {
    return Container(
      decoration: BoxDecoration(
        gradient: LinearGradient(
          colors: [Colors.black, Colors.white],
          begin: Alignment.topCenter,
          end: Alignment.bottomCenter,
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
```
```dart
// Variables
import 'package:flutter/material.dart';
var startAlignment = Alignment.topCenter;
var endAlignment = Alignment.bottomCenter;
class GradientContainer extends StatelessWidget {
  const GradientContainer({super.key});
  @override
  Widget build(BuildContext context) {
    return Container(
      decoration: BoxDecoration(
        gradient: LinearGradient(
          colors: [Colors.black, Colors.white],
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
```
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