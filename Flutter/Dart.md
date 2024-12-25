
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