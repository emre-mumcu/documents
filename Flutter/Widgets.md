
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

# ListView Widget

```dart
// Not Efficient for long list
ListView(children: ) 

// Efficient for long list
// items are built only when they are visible in the screen
ListView.builder(
    itemBuilder: (ctx, index) {
        return ...
    }
)
// or
ListView.builder(
    itemBuilder: (ctx, index) => Text('')
)

// sample

final List<Expense> expenses;

ListView.builder(
    itemCount: expenses.length,
    itemBuilder: (ctx, index) => Text(expenses[index].title)
)

// ListItem

return Card(
    child: Text('Title'),
);

ListView.builder(
    itemCount: expenses.length,
    itemBuilder: (ctx, index) => Card(),
)

// 

Scaffold(
    body: Column(
        children: [
            const Text('Title'),
            Expanded(
                child: ExpenseList()
            )
        ]
    )
)

```

# Card Widget

```dart
Card(
    child: Padding(
        padding: ,
        child: Column(
            children: [
                Text(),
                SizeBox(height: 4),
                Row(
                    children: [
                        Text(),
                        const Spacer(),
                        Row(
                            children: [
                                Icon(Icons.alarm),
                                const SizedBox(width: 8),
                                Text(),
                            ]
                        ),
                    ],
                ),
            ]
        ),
    ),
),
```

# intl

```dart
const categoryIcons = {
    Category.food: Icons.lunch_dining,
    Category.travel: Icons.flight_taleoff,
}

Icon(categoryIcons[0]);

import 'package:intl/intl.dart';

final formatted = DateFormst.yMd();

class Expense {
    // ..

    String get formattedDate {
        return formatter.format(date);
    }


}
```

# Scaffold Widget

```dart
MaterialApp(
    theme: ThemeData(useMaterial3: true),
    home: Scaffold(
        appBar: AppBar(
            title: Text(),
            actions: [
                IconButton(
                    icon: const Icon(Icons.add),
                    onPressed: _open, // () {},
                ),
            ]
        ),
    )
)

void _open() {    
}
```

# showModalBottomSheet

```dart
void _open() {
    showModalBottomSheet(
        useSafeArea: true,
        isScrollControled: true, // modal gets full height
        context: context, builder: (ctx) {
            return Text(''),
        }
    );
}

// Remove overlay:
Navigator.pop(context);
```

# Context

Context keeps metadata related with a widget.

# TextField Widget

```dart
// way 1 handling text input
var _text = '';

void _saveText(String input) {
_text = input;
}

TextField(
    onChanged: _saveText,
    maxLength: 50,
    keyboardType: TextInputType.text,
    decoration: InputDecoration(
        label: Text(''),
    ),    
),
Row(
    children: [
        ElevatedButton(
            onPressed: () {
                print(_text);
            },
            child: Text('Save'),
        )
    ]
)
```

```dart
// way2 handling text input
final _titleController = TextEditingController();

// only state classes can implement dispose method
@override
void dispose() {
    _titleController.dispose();
    super.dispose();
}

TextField(
    controller: _titleController,
    maxLength: 50,
    keyboardType: TextInputType.text,
    decoration: InputDecoration(
        label: Text(''),
    ),
    prefixText: '\$',
),

// print(_titleController.text);
```

# DateInput

```dart
// Row in a Row or Column in a Column requires an Expanded widget to make layout smooth
Expanded(
    child: Row(
        children: [
            Text('Selected Date'),
            IconButton(
                icon: Icon(Icons.calendar_month),
                onPressed: getDate,
            ),

        ]
    )
)

// way 1
void getDate() {
    final now = DateTime.now();
    final first = DateTime(nom.year-1, now.month, nom.day);    
    showDatePicker(
        context: context, initialDate: now, firstDate: first, lastDate: now
    ).then( (value) {

    },);
}

// way 2
void getDate() async {
    final now = DateTime.now();
    final first = DateTime(nom.year-1, now.month, nom.day);    
    final pickedDate = await showDatePicker(
        context: context, initialDate: now, firstDate: first, lastDate: now
    );
    print(pickedDate);
    setState(() {
        _selectedDate = pickedDate;
    });
}
```

# DropdownButton Widget

```dart
MyEnum _selectedEnum = MyEnum.item1;

DropdownButton(
    value: _selectedEnum,
    item: MyEnum.values.map(
        (item) => DropdownMenuItem(
            value: item,
            child: Text(item.name),
        ) 
    ).toList(), 
    onChanged: (value) {
        if(value == null) return;
        setState(() {            
            _selectedEnum = value;
        }); 
    } 
),
```

# Spacer Widget

# showDialog

```dart
showDialog(context: context, builder: (ctx) => 
    AlertDialog(
        title: Text('Invalid'),
        content: Text('Please read this alert!'),
        actions: [
            TextButton(
                onPressed: () {
                    Navigator.pop(ctx);
                },
                child: Text('OK');
            ),
        ],
    ),
);
```

# Dismissable Widget

```dart
Dismissable(
    key: ValueKey(index),
    child: ,
    onDismissed: (direction) {
        // index can be used here
    }
)
```

# ScaffoldMessenger Widget

```dart
ScaffoldMessenger.of(context).clearSnackBars();
ScaffoldMessenger.of(context).showSnackBar(
    SnackBar(
        duration: Duration(seconds: 3),
        content: Text(''),
        action: SnackbarAction(
            label: 'Undo',
            onPressed: () {},
        )
    ),
),
```

# Theming

```dart
void main() {
    return App(
        MaterialApp(            
            // theme: ThemeData(useMaterial3: true), // This creates ThemeData from scratch
            theme: ThemeData().copyWith(useMaterial3: true, //...), // This overrides only provided styles
            home:
        ),
    ),
}
```

## ColorScheme && Text Styles
```dart
void main() {
    return App(
        MaterialApp(
            themeMode: ThemeMode.system, // default            
            theme: ThemeData().copyWith(
                // with colorScheme you can specify one color and flutter can infer and create other shades or variations of colors
                colorScheme: kColorScheme,
                appBarTheme: AppBarTheme.copyWith(
                    backgroundColor.kColorScheme.onPrimaryContainer,
                    foregroundColor: kColorScheme.primaryContainer,
                ),
                cardTheme: CardTheme.copyWith(
                    color: kColorScheme.xxx,
                    margin: EdgeInsets.all(16),
                    margin: EdgeInsets.symmetric(horizontal: 16, vertical: 8),
                ),
                elevatedButtonTheme: ElevatedButtonThemeData(
                    style: ElevatedButton.styleFrom(
                        backgroundColor: kColorScheme.primaryContainer,
                    ),
                ),
                textTheme: ThemeData().textTheme.copyWith(
                    // titleLarge: ThemeData().textTheme.titleLarge.copyWith(),
                    titleLarge: TextStyle(
                        fontWeight: FontWeight.normal,
                        fontSize: 14
                    ),
                ),
            ), 
            darkTheme: ThemeData.dark().copyWith(
                brightness: Brightness.dark,
                colorScheme: kDarkColorScheme,
            ),           
        ),
    ),
}

var kColorScheme = ColorScheme().fromSeed(seedColor: const Color.fromARGB(255,69,59,181));
var kDarkColorScheme = ColorScheme().fromSeed(seedColor: const Color.fromARGB(255,5,99,125));
```

## Using ThemeData in Widgets

```dart
// custom style
Text('', style: TextStyle(),)

// ThemeData style
Text('', style: Theme.of(context).textTheme.titleLarge,),
Text('', style: Theme.of(context).textTheme.titleLarge.copyWith(),),

Container(
    color: Theme.of(context).colorScheme.error.withOpacity(0,75),
    margin: 
),
```

## Dark Mode

```dart

```


# FractionallySizedBox

Document Here

# Responsive and Adaptive Apps

```dart
// Lock Device Orientation:

import 'package:flutter/services.dart';

void main() {
    WidgetsFlutterBinding.ensureInitialized();
    SystemChrome.setPreferredOrientations([
        // Allow only specified orientations
        DeviceOrientation.portraitUp,
    ]).then((fn) {
        runApp();
    });    
}
```

# Widget Size Constraints

**Scaffold**
height-> max. device height
width-> max. device width

**Column**
height-> as much as possible, unconstraint -> INFINITY
width-> as much as needed by children

**Row**
height-> as much as needed by children
width-> as much as possible, unconstraint -> INFINITY

**Expanded**
height-> as much as available
width-> as much as available

# Screen Overlays & Soft Keyboard

```dart
// To get overlaying elements over UI
final keyboardSpace = MediaQuery.of(context).viewInsets.bottom;
```

# LayoutBuilder Widget

```dart
return LayoutBuilder(builder: (ctx, constraints) {
    final width = constraints.width;
    return SizedBox(
        child: Column(
            children: [
                if(width>=600)
                    Row(children: [

                    ],),
            ],
        ),
    ),
}),
```

# showCupertinoDialog

```dart
showCupertinoDialog(context: context, builder: (ctx) => CupertinoAlertDialog(
    
),), 
```

# ff






























