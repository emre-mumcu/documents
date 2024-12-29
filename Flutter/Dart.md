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

# Using "if" Statements In Lists 

in Dart, you may also use if inside of lists to conditionally add items to lists:

final myList = [
  1,
  2,
  if (condition)
    3
];

You can also specify an else case - an alternative value that may be inserted into the list if condition is not met:

final myList = [
  1,
  2,
  if (condition)
    3
  else
    4
];

Alternatively, you could, for example, also work with a ternary expression:

final myList = [
  1,
  2,
  condition ? 3 : 4
];

# Using "for" Loops In Lists

Just as you can also use the if keyword inside of lists (to add elements conditionally), you can also use the for keyword to add multiple items into a list:

final numbers = [5, 6];
final myList = [
  1,
  2,
  for (final num in numbers)
    num
];

When used in a list, it's essentially an alternative to the spread operator (...):

final numbers = [5, 6];
final myList = [
  1,
  2,
  ...numbers
];

It can be useful in scenarios where values must be transformed before being added to a list - the for ... in loop can then be used instead of map() + spread operator:

final numbers = [5, 6];
final myList = [
  1,
  2,
  ...numbers.map((n) {
    return n * 2; 
  }) // adds 10 and 12
];

can be replaced with:

final numbers = [5, 6];
final myList = [
  1,
  2,
  for (final num in numbers)
    num * 2 // adds 10 and 12
];


# _ClassName

Leading _ at the beginning of the class name makes class private.
Leading _ at the name of other elements also makes them (properties, methods) private.

# Getters

```dart
List<Map<String, Object>> get summaryData {
    final List<Map<String, Object>> summary = [];
    // ...
    return summaryData;
}
```

# Arrow Functions

```dart
// () {}; Anonymous Function
// () => true; Arrow Function

```

# Debugging

Open panel in VSCode:
View -> Appearance -> Panel
and select DebugConsole

Debug Mode:
Start App with Debugging mode. Add breakpoints.

Flutter Developer Tools:
VSCode: Open command palette and search Flutter: Open DevTools and select Oprn DevTools in Web Browser

# Initializer Lists

Initializer Lists can be used to initialize class properties with values that are NOT received as constructor function args.

```dart
const final uuid = Uuid();

class Expense {
    Expense({required this.title}) : id = uuid.v4();

    final String id;
    final String title;
}
```

# Enums

```dart
enum Category { cat1, cat2, cat3 }

final Category category;
```

# ss































































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