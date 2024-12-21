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
