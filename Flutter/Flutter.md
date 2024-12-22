# Flutter Development

[Flutter](https://github.com/flutter/flutter) is an open-source UI toolkit (framework) by Google for building natively compiled applications for mobile, web, and desktop from a single codebase. It uses the Dart programming language and provides fast development with features like hot reload.

User interfaces are built with [widgets](https://docs.flutter.dev/ui/widgets). Flutter uses [material design](https://m3.material.io/).

## Flutter Setup

Your [Flutter Development Environment](https://docs.flutter.dev/get-started/install/windows/mobile) must have the following components:

* [Flutter SDK](https://docs.flutter.dev/get-started/install)
* [Git](https://git-scm.com/)
* [Android Studio](https://developer.android.com/studio)
* A Virtual Device

1. To install Flutter, download the [Flutter SDK bundle](https://docs.flutter.dev/release/archive) from its archive, move the bundle to where you want it stored, then extract the SDK. Don't install Flutter to a directory or path that meets one or both of the following conditions:
    * The path contains special characters or spaces.
    * The path requires elevated privileges.
2. To run Flutter commands in PowerShell, add Flutter (**bin folder**) to the PATH environment variable.
3. Download and install Android Studio.
4. To create Android apps with Flutter, verify that the following Android components have been installed. Use Android Studio to install the following components:
    * Android SDK Platform
    * Android SDK Command-line Tools
    * Android SDK Build-Tools
    * Android SDK Platform-Tools
    * Android Emulator
    To install these components
5. Configure your target Android device.
6. Before you can use Flutter and after you install all prerequisites, agree to the licenses of the Android SDK platform.
```bat
    flutter doctor --android-licenses
```
7. Check your development setup. The flutter doctor command validates that all components of a complete Flutter development environment for Windows.
```bat   
    flutter doctor
```    
    When the flutter doctor command returns an error, it could be for Flutter, VS Code, Android Studio, the connected device, or network resources. If the flutter doctor command returns an error for any of these components, run it again with the verbose flag.
```bat    
    flutter doctor -v
```

If you have warnings about android sdk or android studio, you can configure them using the following commands:

```bat
flutter config --android-sdk "C:\android-sdk"
flutter config --android-studio-dir "C:\android-studio"
```

After setting up your development environment, you can verify the components running the following commands:

```bash
$ dart --version
$ flutter --version
```

## Flutter and Dart Plugins

You need to install Flutter and Dart plugins for Android Studio or VSCode to create flutter applications. 

After installing flutter extension to VSCode, you can access flutter commands by typing Flutter to command bar (F1 to open command bar).

Flutter: Launch Emulator

Settings -> Languages & Frameworks -> Flutter and check Format code on save.
Editor > General > Appearance > Show Closing Labels in Dart source code

## Create First Application

Run the following commands to create and run your first flutter application. Do NOT use spaces and dash character in folder names.

```bash
$ flutter create first_app
$ flutter run
```

[Flutter Source Code](https://github.com/flutter/flutter/tree/master/packages/flutter/lib/src)