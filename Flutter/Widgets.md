
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
    return Scaffold();
  }
}
```

Since the build method is overriden in ClassState, it will be called whenever there is a change in the variables associated with the Widgets present in it and the whole widget will be redrawn once again. In order to notify the state change event, we need to call `setState()` method that will trigger the build method.

## Stateful Widget Lifecycle:

Every Flutter Widget has a built-in lifecycle: A collection of methods that are automatically executed by Flutter (at certain points of time).

There are three extremely important stateful widget lifecycle methods you should be aware of:

1. initState(): Executed by Flutter when the StatefulWidget's State object is initialized
2. build(): Executed by Flutter when the Widget is built for the first time AND after setState() was called
3. dispose(): Executed by Flutter right before the Widget will be deleted (e.g., because it was displayed conditionally)

# Container Widget

Container supports decoration.

```dart
Container(
    decoration: const BoxDecoration(
        gradient: LinearGradient(
            colors: [
                Color.fromARGB(),
                Color.fromARGB(),
            ],
        ),
        begin: Alignment.topLeft,
        end: Alignment.bottomRight,
        child: MyWidget(),
    ),
    margin: EdgeInsets...
),
```

# Center Widget

Occupies as much space as it can.

In Center widgest, child widgets have mainAxisSize: MainAxisSize.min etc. to comply with positioning and become centered

# Column - Row Widgets

Supports multiple widgets

Column(    
    mainAxisAlignment: MainAxisAlignment.center,
    crossAxisAlignment: CrossAxisAlignment.stretch,
)

# Image Widgets

Image.asset('assets/images/my.png', width: XXX);

TRansparent Image:

way 1:

Opacity( // perf intensive
    opacity: 0.5
    child: Image.asset('assets/images/my.png', width: XXX);
)

way 2:
// perf optimal and preferred way with color
 Image.asset('assets/images/my.png', width: XXX, color: Color.fromARGB(150,255,255,255));


# SizedBox Widget

const SizedBox(height: XX)
width: double.infinity -> entire width
height: 300
SizedBox cuts out the content which is beyond its size.

# Text Widget

const Text('', style: TextStyle()),

# Button Widget

Button: 

OutlinedButton

onPressed: () {} : Anonymous function
child: Text('ButtonText')
style: OutlinedButton.styleFrom(
    foregroundColor: Colors.white
)

Button with icon:

OutlinedButton.icon(
    onPressed: () {} : Anonymous function
    label: Text('ButtonText')
    icon: const Icon(Icons.arrow_right_alt),
    style: OutlinedButton.styleFrom(
        foregroundColor: Colors.white
    )
)

# Navigation 1

onPressed: () { }

```dart
// quiz.dart
// state + state manipulation
import 'package:flutter/material.dart';

class Quiz extends StatefulWidget {
    const Quiz({super.key});

    @override
    State<Quiz> createState() {
        return _QuizSate();
    }
}

// starts from _ makes private
class _QuizSate extends State<Quiz> {

    Widget? activeSCreen;

    @override
    void initState() { // Extra initialization, will executed ONCE, will execute before the build method
        activeSCreen = const StartScreen(switchScreen);
        super.initState();
    }

    

    switchScreen() {
        // flutter will re-execute build method 
        setState(
            () {
                activeSCreen = const QuestionScreen();
            }
        );
    }

    @override
    Widget build(context) {
        return MaterialApp(
            child: activeSCreen
        )
    }
}

```

```dart

// questions_screen.dart

import 'package:flutter/material.dart';

class QuestionScreen extends StatefulWidget {
    const QuestionScreen({super.key});

    @override
    State<QuestionScreen> createState() {
        return _QuestionScreenSate();
    }
}

// starts from _ makes private
class _QuestionScreenSate extends State<QuestionScreen> {
    @override
    Widget build(context) {
        return Text()
    }
}

```

```dart
// start_screen.dart

import 'package:flutter/material.dart';

class StartScreen extends StatelessWidget {
    const StartScreen(this.switchScreenFn, {super.key});

    final void Function() switchScreenFn;

    @override
    Widget build(context) {
        return Center(
            child: OutlinedButton(
                onPressed: () {
                    switchScreenFn();
                }
                // or
                onPreesed: switchScreenFn
            )
        )
    }
}



```

# Navigation 2

onPressed: () { }

```dart
// quiz.dart
// state + state manipulation
import 'package:flutter/material.dart';

class Quiz extends StatefulWidget {
    const Quiz({super.key});

    @override
    State<Quiz> createState() {
        return _QuizSate();
    }
}

// starts from _ makes private
class _QuizSate extends State<Quiz> {

    var activeSCreen = 'start-screen';

    switchScreen() {
        // flutter will re-execute build method 
        setState(
            () {
                activeSCreen = 'question-screen';
            }
        );
    }

    @override
    Widget build(context) {
        return MaterialApp(
            child: activeSCreen== 'start-screen' ?
                StartScreen(switchScreen) : const QuestionScreen()
                ;
        )
    }
}

```

```dart
// start_screen.dart

import 'package:flutter/material.dart';

class StartScreen extends StatelessWidget {
    const StartScreen(this.switchScreenFn, {super.key});

    final void Function() switchScreenFn;

    @override
    Widget build(context) {
        return Center(
            child: OutlinedButton(
                onPressed: () {
                    switchScreenFn();
                }
                // or
                onPreesed: switchScreenFn
            )
        )
    }
}



```


# Navigation 3

# CustomButton

```dart
class AnswerButton extens StatelessWidget 
{
    // Positional Arguments
    const AnswerButton(this.text, this.onTap, {super.key});

    // Named Arguments (named arguments are optional by default)
    const AnswerButton({super.key, required this.text, required this.onTap});

    final String text;
    final void Function() onTap;

    @override
    Widget build(BuildContext context) {
        return ElevatedButton(
            onPressed: () {},
            // style: ElevatedButton.styleFrom(),
            style: ElevatedButton.styleFrom(
                padding: EdgeInsets.all(10),                
                padding: EdgeInsets.symmetric(vertical: 10, horizontal: 40),                
                backgroundColor: ...,
                foregroundColor: ...,
                shape: RoundedRectangleBorder(
                    borderRadius: BorderRadius.circular(40),
                ),
            ),
            child: Text(text),
        );
    }
}

// Use:
AnswerButton('', () {}),
AnswerButton(text: '', onTap: () {}),

```

# Spreading Values (...)

```dart
const numbers = [1,2,3];

const moreNumbers = [numbers, 4]; // [[1,2,3],4]
const moreNumbers = [...numbers, 4]; // [1,2,3,4]


```

# Google Fonts

flutter pub add google_fonts

import 'package:google_fonts/google_fonts.dart';

Text('', style: GoogleFonts.lato(
    color: 
    fontSize:
    fontWeight:
)),

# Expanded Widget

Expanded widgets aligns its childs widgets size to its parent flex widget size.

Row -> Expanded -> Column

Max width of a Column is screen width and max height of a Row is screen height.

# SingleChildScrollView

SizedBox -> SingleChildScrollView

# f



