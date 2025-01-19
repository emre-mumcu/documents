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

## Configuration

Get Flutter Global Config Values:

flutter config --list

To reset configurations to their default state, you can delete or modify the ~/.flutter_settings file, or run:

flutter config --clear-features

You can configure flutter using the following commands:

```bat
flutter config --android-sdk /path/to/android/sdk
flutter config --android-studio-dir /path/to/android/studio
flutter config --jdk-dir=<JDK_DIRECTORY>

flutter config --android-sdk "C:\android-sdk"
flutter config --android-studio-dir "C:\android-studio"
flutter config --jdk-dir="C:\Java\jdk-23\"

flutter config --enable-linux-desktop
flutter config --enable-macos-desktop
flutter config --enable-windows-desktop
flutter config --no-enable-web
```

Or add a new environment variable ANDROID_HOME with your Android SDK path.

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

## Custom Gradle Path

Gradle Build Tool is a fast, dependable, and adaptable open-source build automation tool with an elegant and extensible declarative build language.

Gradle runs on the Java Virtual Machine (JVM), which is often provided by either a JDK or JRE. A JVM version between 8 and 23 is required to execute Gradle. JVM 24 and later versions are not yet supported.

Executing the Gradle daemon with JVM 16 or earlier has been deprecated and will become an error in Gradle 9.0.

If you are working on a Gradle project, you can check the version of Gradle used by the wrapper. Locate the gradle/wrapper/gradle-wrapper.properties file in the project directory. Open the file and look for a line containing gradle version.

The gradle used by wrapper can be found in the following path in Flutter application:

```text
APPNAME\android\gradle\wrapper\gradle-wrapper.properties
```
See
[Compatibility Matrix](https://docs.gradle.org/current/userguide/compatibility.html#java)

[Gradle Wrapper](https://docs.gradle.org/current/userguide/gradle_wrapper.html#sec:upgrading_wrapper)

## How to completely uninstall Android Studio

1. Run the Android Studio uninstaller: The first step is to run the uninstaller. Open the Control Panel and under Programs, select Uninstall a Program. After that, click on "Android Studio" and press Uninstall. If you have multiple versions, uninstall them as well.
2. Remove the Android Studio files: To delete any remains of Android Studio setting files, in File Explorer, go to your user folder `%USERPROFILE%`, and delete `.android`, `.AndroidStudio` and any analogous directories with versions on the end, i.e. `.AndroidStudio1.2`, `.gradle` and `.m2` if they exist.
3. Also delete the any `AndroidStudio*` directories that are in `%LOCALAPPDATA%\Google` and `%APPDATA%\Google`.
4. Finally, go to `C:\Program Files` and delete the `Android` directory.
5. To delete any remains of the SDK, go to `%LOCALAPPDATA%` and delete the Android directory.

**Summary of all paths of leftover files from Android Studio**

* C:\Program Files\Android
* C:\Users\USERNAME\AppData\Local\Android
* C:\Users\USERNAME\AppData\Local\Google\AndroidStudio2024.2
* C:\Users\USERNAME\AppData\Roaming\Google\AndroidStudio2024.2
* C:\Users\USERNAME\.gradle
* C:\Users\USERNAME\.android
* C:\Users\USERNAME\.AndroidStudio
* C:\Users\USERNAME\.konan
* C:\Users\USERNAME\.m2

# Flutter Source Code

[Flutter Source Code](https://github.com/flutter/flutter/tree/master/packages/flutter/lib/src)