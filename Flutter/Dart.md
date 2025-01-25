# Constructor Parameters

In Dart, parameters in constructors can be categorized into positional and named parameters.

## 1. Positional Parameters:

Positional Parameters are provided in the order they are defined. They are required unless made optional using square brackets ([]).

```dart
class ClassName {
  ClassName(parameter1, parameter2, parameter3='', [parameter4]) {
    // Initialization or logic
  }
}

```

## 2. Named Parameters {}:
Named parameters allow you to pass arguments using parameter names instead of relying on their order. This improves readability.

```dart
class MyClass {
  String name;
  int age;
  // Constructor with named parameters
  MyClass({required this.name, required this.age});
  // Constructor with named optional parameters
  MyClass({this.name = 'Unknown', this.age = 0});  
}
```
Named parameters can be optional or required based on whether you specify a default value or mark them with required.

required ensures that the parameters must be provided when creating the object. If you omit a required parameter, Dart will throw a compile-time error.

## 3. Optional Parameters []:
Optional parameters can either be positional or named. These parameters are not required when calling a constructor.

```dart
// Optional Positional Parameters
class ClassName {
  ClassName([parameter1, parameter2]) {
    // Initialization or logic
  }
}

// Optional Named Parameters
class ClassName {
  ClassName({parameter1, parameter2}) {
    // Initialization or logic
  }
}
```

## 4. Default Parameters

Default parameters (available in both optional named and optional positional parameters) allow you to assign default values to parameters. If no argument is passed, the default value is used.

```dart
// Default Values in Positional Optional Parameters:
class Person {
  String name;
  int age;

  // Constructor with default values
  Person([this.name = 'Unknown', this.age = 18]);
}

void main() {
  var person = Person();  // Uses default values
  print(person.name);  // Outputs: Unknown
  print(person.age);   // Outputs: 18
}


// Default Values in Named Optional Parameters:
class Person {
  String name;
  int age;

  // Constructor with default values for named parameters
  Person({this.name = 'Unknown', this.age = 18});
}

void main() {
  var person = Person();  // Uses default values
  print(person.name);  // Outputs: Unknown
  print(person.age);   // Outputs: 18
}
```

## Comparison

|Type|Syntax|Required?|Order Matters?|Default Value Support?|
|---|---|---|---|---|
|Positional|(parameter1, parameter2)|Yes|Yes|No|
|Named|({required parameter1, required parameter2})|Yes|No|Yes (if not required)|
|Optional Positional|([parameter1, parameter2])|No|Yes|Yes|
|Optional Named|({parameter1, parameter2})|No|No|Yes|

# Named Constructors

In Dart, named constructors are a way to provide multiple ways to create instances of a class with different initialization logic. Named constructors allow you to define additional constructors in a class, each with a different name. This is useful for providing more flexibility and readability in object creation.

```dart
// template
class MyClass {
  String name;
  
  // Default constructor
  MyClass(this.name);

  // Named constructor
  MyClass.namedConstructor(this.name);
}

// Named Constructors for Custom Initialization Logic
class Rectangle {
  double width;
  double height;

  // Default constructor
  Rectangle(this.width, this.height);
  
  // Named constructor for square
  Rectangle.square(double side) : width = side, height = side;

  double area() {
    return width * height;
  }
}

void main() {
  var rect1 = Rectangle(5, 10);  // Regular rectangle
  var square = Rectangle.square(5);  // Square with equal sides

  print(rect1.area());  // Outputs: 50
  print(square.area());  // Outputs: 25
}

// Named Constructor with Additional Parameters
class Person {
  String name;
  int age;

  // Default constructor
  Person(this.name, this.age);

  // Named constructor
  Person.createFromMap(Map<String, dynamic> data)
      : name = data['name'],
        age = data['age'];

  void introduce() {
    print('Hello, my name is $name and I am $age years old.');
  }
}

void main() {
  var person1 = Person('Alice', 30);  // Using default constructor
  
  var personData = {'name': 'Bob', 'age': 25};
  var person2 = Person.createFromMap(personData);  // Using named constructor
  
  person1.introduce();  // Outputs: Hello, my name is Alice and I am 30 years old.
  person2.introduce();  // Outputs: Hello, my name is Bob and I am 25 years old.
}
```

# Singleton Pattern

A singleton is a design pattern where only one instance of a class is created, and this instance is reused throughout the application.

```dart
class Singleton {
  // Private static field to store the single instance
  static final Singleton _instance = Singleton._internal();

  // Private constructor
  Singleton._internal();

  // Factory constructor returning the single instance
  factory Singleton() {
    return _instance;
  }

  void doSomething() {
    print('Doing something');
  }
}

void main() {
  var s1 = Singleton();
  var s2 = Singleton();
  
  print(s1 == s2);  // Outputs: true, both variables refer to the same instance
}
```

# factory Keyword

In Dart, the factory keyword is used to define a factory constructor. A factory constructor is a special kind of constructor that is responsible for returning an instance of a class, but it doesn't always create a new instance directly. Instead, it can decide whether to create a new object or return an existing one, or even return an instance of a subclass.

```dart
class MyClass {
  // Factory constructor
  factory MyClass() {
    // Custom logic for instance creation
    return MyClass._internal();
  }

  // Private named constructor
  MyClass._internal();

  // Other properties and methods
}
```

For example:

```dart
class Car {
  final String model;
  
  // Factory constructor
  factory Car(String model) {
    if (model == 'Tesla') {
      return Tesla();  // Returns an instance of a subclass (Tesla)
    } else {
      return Car._(model);  // Returns the normal Car instance
    }
  }

  // Private constructor
  Car._(this.model);

  void drive() {
    print('Driving $model');
  }
}

class Tesla extends Car {
  Tesla() : super._('Tesla');
  
  @override
  void drive() {
    print('Driving Tesla');
  }
}

void main() {
  var myCar = Car('Tesla');
  myCar.drive();  // Outputs: Driving Tesla
  
  var anotherCar = Car('BMW');
  anotherCar.drive();  // Outputs: Driving BMW
}
```

# with Keyword

In Dart, the with keyword is used to apply mixins to a class. A mixin is a way to add functionality to a class without using inheritance. This allows you to reuse code across multiple classes, which is more flexible than traditional inheritance.

When you use with, you're combining the functionality of another class (or classes) into your current class. It’s similar to multiple inheritance but in a more controlled way, because Dart only allows a class to extend one superclass but can apply multiple mixins.

```dart
class MyClass with Mixin1, Mixin2 {
  // MyClass now has all the functionality from Mixin1 and Mixin2
}
```

For example, the Diagnosticable mixin enables a class to include additional properties or methods that help with diagnostics (like a readable toString representation).

```dart
class ThemeData with Diagnosticable {
  // Your properties and methods for ThemeData
}
```

# import

In Dart, the import statement is used to bring external libraries, packages, or files into the current file, making their classes, functions, and other resources available for use.

```dart
// Importing Libraries
import 'dart:math';
// Importing a file or package 
// You can import packages that you’ve added to your project
import 'package:some_package/some_file.dart';
// You can selectively import specific elements from a library using show
import 'dart:math' show Random;
import 'dart:ui' show Color, lerpDouble;
// If you want to import a library but exclude certain parts, you can use hide
import 'dart:math' hide Random;
// You can also import files within your project using a relative path
import 'utils/helper.dart';
// If you’re importing a large library or multiple libraries with the same class names, you can use as to avoid conflicts:
import 'package:math_tools/math_tools.dart' as math;
import 'package:geometry_tools/geometry_tools.dart' as geom;
```


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

# Future Object

Document here...

# for - in

```dart
for (final expense in expenses) {
}
```

# Named Constructor

```dart
class ExpenseBucket {
    const ExpenseBucket({
        required this.category, required this.expenses
    });

    // Named constructor function:
    ExpenseBucket.forCategory(List<Expenses> allExpenses, this.category) 
        : expenses = allExpenses.where(e => e.category == category).toList();

    final Category category;
    final List<Expense> expenses;
}
```

# MediaQuery

```dart
final isDarkMode = MediaQuery.of(context).platformBrightness == Brightness.dark;
var width = MediaQuery.of(context).size.width;
var platform = Platform.IsIOS;
```

# gg































































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