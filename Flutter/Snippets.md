```dart
// StatefulWidget
class YellowBird extends StatefulWidget {
  const YellowBird({ super.key });
  @override
  State<YellowBird> createState() => _YellowBirdState();
}

class _YellowBirdState extends State<YellowBird> {
  @override
  void initState() {
    super.initState();
    // Using WidgetsBinding to delay the execution 
    // until the widget is fully initialized
    WidgetsBinding.instance.addPostFrameCallback((_) {
        // ...
      });
    });
  }
  @override
  Widget build(BuildContext context) {
    return Container(color: const Color(0xFFFFE306));
  }
}
```


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