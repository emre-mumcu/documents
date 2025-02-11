# FAILURE: Build failed with an exception.

You may get the following error while running the flutter application:

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

You can manually edit the following files and update the versions for Gradle: 

* android/app/build.gradle 
* android/gradle/wrapper/gradle-wrapper.properties
* android/settings.gradle.


`android\app\build.gradle`

```text
android 
{
    ndkVersion "25.1.8937393"

    compileOptions 
    {
        sourceCompatibility JavaVersion.VERSION_17
        targetCompatibility JavaVersion.VERSION_17
    }

    kotlinOptions 
    {
        jvmTarget = 17
    }
}
```

`android/settings.gradle`

```text
id "com.android.application" version "8.3.2" apply false
id "org.jetbrains.kotlin.android" version "2.0.20" apply false
```

`android/gradle/wrapper/gradle-wrapper.properties`

```text
distributionUrl=https\://services.gradle.org/distributions/gradle-8.10.2-all.zip
```

# References

* https://github.com/flutter/flutter/issues/156304