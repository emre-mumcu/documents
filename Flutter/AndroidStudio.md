https://developer.android.com/tools/variables

# Android Studip & Flutter Development Environment

To set up Android Studio and Flutter development, you need to configure several environment variables for Flutter, the Android SDK, and Java. Here’s a complete list:

## 1. Android Studio & SDK Path

Android Studio path:

ANDROID_STUDIO_HOME (for some configurations)
STUDIO_HOME (less common)

Android SDK path:

ANDROID_HOME (Deprecated but still used by some tools. Replaced by ANDROID_SDK_ROOT. Don’t use ANDROID_HOME unless you have to.)
ANDROID_SDK_HOME (Deprecated and it is no longer recommended. Controls where Android emulator settings like .android folder are stored. Use ANDROID_SDK_HOME if you want emulator settings in a custom location.)
ANDROID_SDK_ROOT (Recommended for new setups)

The path to the user preferences directory for tools that are part of the Android SDK. Defaults to $HOME/.android/ :

ANDROID_USER_HOME

## 2. Flutter SDK Path

Needed to run Flutter commands globally.

FLUTTER_HOME

## 3. Java Path

Needed for Gradle and Android Studio to work properly.

JAVA_HOME

The location of the JDK that Android Studio runs in. When you launch the IDE, it checks the STUDIO_JDK, JDK_HOME, and JAVA_HOME environment variables, in that order.

STUDIO_JDK

Sets the location of the JDK that Android Studio uses to start the Gradle daemon. When you launch the IDE, it first checks STUDIO_GRADLE_JDK. If STUDIO_GRADLE_JDK is not defined, the IDE uses the value set in the project structure settings.

STUDIO_GRADLE_JDK

## 4. AVD Path (If Using a Custom Location)

Defines where Android Virtual Devices (AVDs) are stored. By default, Android Studio stores AVD (Android Virtual Device) files in a system directory, but you can change this location by setting the ANDROID_AVD_HOME and ANDROID_SDK_HOME environment variables.

ANDROID_AVD_HOME

Sets the path to the user-specific emulator configuration directory. Defaults to $ANDROID_USER_HOME

ANDROID_EMULATOR_HOME

Older tools, such as Android Studio 4.3 and earlier, do not read ANDROID_USER_HOME. For those tools, the default value is $ANDROID_SDK_HOME/.android.

## 5. Gradle Path (If Using a Custom Version)

GRADLE_HOME

## Example

```
| Variable Name | Value |
| ---- | ---- |
|ANDROID_STUDIO_HOME|C:\Android\studio|
|ANDROID_HOME|C:\Android\sdk|
|ANDROID_SDK_ROOT|C:\Android\sdk|
|ANDROID_AVD_HOME|S:\AVD|
|ANDROID_EMULATOR_HOME|S:\AVD|
|ANDROID_USER_HOME|S:\.android|
|JAVA_HOME|C:\Android\jdk-17|
|FLUTTER_HOME|C:\Android\flutter|
```

Add the following entries to the PATH:

* %ANDROID_HOME%\tools
* %ANDROID_HOME%\tools\bin
* %ANDROID_HOME%\platform-tools
* %JAVA_HOME%\bin
* %FLUTTER_HOME%\bin

## Verify Setup

After setting environment variables, and installing the required software restart your terminal and verify:

```
flutter doctor
```

# JetBrains Runtime (JBR)
The JetBrains Runtime (JBR)  is an enhanced JDK, distributed with Android Studio. It includes several optimizations for use in Studio and related JetBrains products, but can also be used to run other Java applications.

# How do I choose a JDK to run Android Studio?
We recommend that you use the JBR to run Android Studio. It's deployed with and used to test Android Studio, and includes enhancements for optimal Android Studio usage. To ensure this, don't set the STUDIO_JDK environment variable.

The startup scripts for Android Studio look for a JVM in the following order:

1. STUDIO_JDK environment variable
2. studio.jdk directory (in the Android Studio distribution)
3. jbr directory (JetBrains Runtime), in the Android Studio distribution. Recommended.
4. JDK_HOME environment variable
5. JAVA_HOME environment variable
6. java executable in the PATH environment variable

# How do I choose which JDK runs my Gradle builds?
If you run Gradle using the buttons in Android Studio, the JDK set in the Android Studio settings is used to run Gradle. If you run Gradle in a terminal, either inside or outside Android Studio, the JAVA_HOME environment variable (if set) determines which JDK runs the Gradle scripts. If JAVA_HOME is not set, it uses the java command on your PATH environment variable.

For the most consistent results, make sure you set your JAVA_HOME environment variable, and Gradle JDK configuration in Android Studio to that same JDK.

# Gradle JDK configuration in Android Studio
To modify the existing project's Gradle JDK configuration, open the Gradle settings from File (or Android Studio on macOS) > Settings > Build, Execution, Deployment > Build Tools > Gradle.

Be aware that there are multiple JDKs in play:

1. JDK that runs Android Studio - let it default to the JetBrains Runtime (JBR) that comes with Android Studio

2. JDK that runs Gradle - set it in Android Studio for Studio builds or via org.gradle.java.home in gradle.properties, which defaults to JAVA_HOME, for shell builds

3. Toolchain JDK - default for source and target levels, as well as JDK used to run unit tests. I strongly recommend using this instead of sourceCompatibility, targetCompatibility and jvmTarget. 

