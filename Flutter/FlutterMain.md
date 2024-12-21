```dart
import 'package:flutter/material.dart';

void main() {
  runApp(MaterialApp());
}
```


```dart
import 'package:flutter/material.dart';

void main() {
  runApp(MaterialApp(
    home: Text("Welcome"),
  ));
}
```

```dart
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
