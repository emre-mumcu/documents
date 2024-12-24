# Create Your Flutter Package

## Create a New Package

```bash
flutter create --template=package mmc_tools
```

## Develop Your Package

Implement your functionality in `lib/my_package_name.dart` or create additional Dart files.

Add proper comments and API documentation using DartDoc syntax `(///)`.

Write tests in the test/ directory to ensure your package works as expected.

## Prepare for Publishing

1. Update `pubspec.yaml` to provide metadata for your package:

```yaml
name: my_package_name
description: A brief description of your package.
version: 1.0.0
homepage: https://github.com/yourusername/my_package_name
environment:
  sdk: ">=2.17.0 <3.0.0"
dependencies:
  flutter:
    sdk: flutter
dev_dependencies:
  flutter_test:
    sdk: flutter
```

2. Create a Flutter app in the `example/` directory to demonstrate how to use your package.

```bash
# Navigate to the example/ folder and create a demo Flutter app to showcase how to use your package. Run:
flutter create .
# Then modify the lib/main.dart file to import and use your package
```

3. Write Documentation

* README.md: Include detailed usage instructions, examples, and screenshots.
* CHANGELOG.md: Document changes for each version.
* LICENSE: Add a valid open-source license (e.g., MIT License).

4. Test your package locally:

```bash
flutter test
```

5. Check for issues using dart pub publish dry run:

```bash
dart pub publish --dry-run
```

6. Publish to pub.dev

Log in to your pub.dev account:

```bash
dart pub login
```

Logout:

```bash
dart pub logout
```

7. Publish your package

```bash
dart pub publish
```
