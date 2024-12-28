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
),
```

# Center Widget

Occupies as much space as it can.

In Center widgest, child widgets have mainAxisSize: MainAxisSize.min etc. to comply with positioning and become centered

# Column - Row Widgets

Supports multiple widgets

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
