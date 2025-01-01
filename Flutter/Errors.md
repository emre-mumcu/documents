
# 1. Launch Error

```text
Launching lib\main.dart on sdk gphone64 x86 64 in debug mode...

FAILURE: Build failed with an exception.

* What went wrong:
Execution failed for task ':path_provider_android:compileDebugJavaWithJavac'.
> Could not resolve all files for configuration ':path_provider_android:androidJdkImage'.
   > Failed to transform core-for-system-modules.jar to match attributes {artifactType=_internal_android_jdk_image, org.gradle.libraryelements=jar, org.gradle.usage=java-runtime}.
      > Execution failed for JdkImageTransform: S:\Android\sdk\platforms\android-34\core-for-system-modules.jar.
         > Error while executing process S:\Android\android-studio\jbr\bin\jlink.exe with arguments {--module-path C:\Users\Emre\.gradle\caches\transforms-3\90091773555bbba13f65780d4475003e\transformed\output\temp\jmod --add-modules java.base --output C:\Users\Emre\.gradle\caches\transforms-3\90091773555bbba13f65780d4475003e\transformed\output\jdkImage --disable-plugin system-modules}

* Try:
> Run with --stacktrace option to get the stack trace.
> Run with --info or --debug option to get more log output.
> Run with --scan to get full insights.
> Get more help at https://help.gradle.org.

BUILD FAILED in 7s

┌─ Flutter Fix ────────────────────────────────────────────────────────────────────────────────────┐
│ [!] This is likely due to a known bug in Android Gradle Plugin (AGP) versions less than 8.2.1,   │
│ when                                                                                             │
│   1. setting a value for SourceCompatibility and                                                 │
│   2. using Java 21 or above.                                                                     │
│ To fix this error, please upgrade your AGP version to at least 8.2.1. The version of AGP that    │
│ your project uses is likely defined in:                                                          │
│ C:\AppStore\Dev.Flutter\udemy\meals_app\android\settings.gradle,                                 │
│ in the 'plugins' closure (by the number following "com.android.application").                    │
│  Alternatively, if your project was created with an older version of the templates, it is likely │
│ in the buildscript.dependencies closure of the top-level build.gradle:                           │
│ C:\AppStore\Dev.Flutter\udemy\meals_app\android\build.gradle,                                    │
│ as the number following "com.android.tools.build:gradle:".                                       │
│                                                                                                  │
│ For more information, see:                                                                       │
│ https://issuetracker.google.com/issues/294137077                                                 │
│ https://github.com/flutter/flutter/issues/156304                                                 │
└──────────────────────────────────────────────────────────────────────────────────────────────────┘
Error: Gradle task assembleDebug failed with exit code 1

Exited (1).
```
Since you are on Android Studio (version 2024.2) which ships with Java sdk 21 could be the factor as why you are seeing this error.

jdk 21 is creating the problem

after downgrade jdk to 17 it started to work again

Gradle Settings: 

* android/app/build.gradle 
* android/gradle/wrapper/gradle-wrapper.properties
* android/settings.gradle.



download jdk-11 and install it
then you should see jdk-11 in your C:\Program Files\Java directory
then in the flutter project tirminal run this command flutter config --jdk-dir "C:\Program Files\Java\jdk-11"
This command tells Flutter to use the specified JDK directory instead of relying on the JAVA_HOME environment variable.


I spent several hours trying all combinations, JDK 11, 17, the one from a Sandbox-installed AS Koala but finally I found that the new bundled JDK 21 can be made to work if all the other settings are correct (but this depends on plugins as well, so some of your dependent plugins might need fixing, too — the ones I rely on happened to be either all right or my own that I could fix).

Basically, you need this in app\build.gradle:

```json
android {
  ndkVersion "25.1.8937393"

compileOptions {
  sourceCompatibility JavaVersion.VERSION_17
  targetCompatibility JavaVersion.VERSION_17
}
kotlinOptions {
  jvmTarget = 17
}
```

This in settings.gradle:

```json
id "com.android.application" version "8.3.2" apply false
id "org.jetbrains.kotlin.android" version "2.0.20" apply false
```

And this in gradle-wrapper.properties:

```json
distributionUrl=https\://services.gradle.org/distributions/gradle-8.10.2-all.zip
```

This works, plus, change ndkVersion to 25.1 as the compiler warning indicates:

```json
android {
namespace = "com.example.untitled2"
compileSdk = flutter.compileSdkVersion
ndkVersion = "25.1.8937393"
}
```

https://github.com/flutter/flutter/issues/156304
