# Android Studio JDK and Gradle Configuration

Gradle Build Tool is a fast, dependable, and adaptable open-source build automation tool with an elegant and extensible declarative build language.

Gradle runs on the Java Virtual Machine (JVM), which is often provided by either a JDK or JRE. A JVM version between 8 and 23 is required to execute Gradle. JVM 24 and later versions are not yet supported.

Executing the Gradle daemon with JVM 16 or earlier has been deprecated and will become an error in Gradle 9.0.

If you are working on a Gradle project, you can check the version of Gradle used by the wrapper. Locate the `android\gradle/wrapper/gradle-wrapper.properties` file in the project directory. Open the file and look for a line containing gradle version.

[Compatibility Matrix](https://docs.gradle.org/current/userguide/compatibility.html#java)

[Gradle Wrapper](https://docs.gradle.org/current/userguide/gradle_wrapper.html#sec:upgrading_wrapper)

# Android Studio Java Development Kit

The JetBrains Runtime (JBR) is an enhanced JDK, distributed with Android Studio. It's deployed with and used to test Android Studio, and includes enhancements for optimal Android Studio usage. To ensure using the JBR instead of a JDB, don't set the STUDIO_JDK environment variable.

The startup scripts for Android Studio look for a JVM in the following order:

1. STUDIO_JDK environment variable
2. studio.jdk directory (in the Android Studio distribution)
3. jbr directory (JetBrains Runtime), in the Android Studio distribution. (Recommended)
4. JDK_HOME environment variable
5. JAVA_HOME environment variable
6. java executable in the PATH environment variable

If you run Gradle using the Android Studio UI, the JDK set in the Android Studio settings is used to run Gradle. 

If you run Gradle in a terminal, either inside or outside Android Studio, the JAVA_HOME environment variable (if set) determines which JDK runs the Gradle scripts. If JAVA_HOME is not set, it uses the java command on your PATH environment variable.

For the most consistent results, make sure you set your JAVA_HOME environment variable and Gradle JDK configuration in Android Studio to the same JDK.

# Gradle JDK Configuration in Android Studio

To modify the existing project's Gradle JDK configuration, open the Android Studio and select

* File > Settings > Build, Execution, Deployment > Build Tools > Gradle (Windows)
* Android Studio > Settings > Build, Execution, Deployment > Build Tools > Gradle (MacOS)

Be aware that there are multiple JDKs in play:

1. JDK that runs Android Studio - let it default to the JetBrains Runtime (JBR) that comes with Android Studio
2. JDK that runs Gradle - set it in Android Studio for Studio builds or via org.gradle.java.home in gradle.properties, which defaults to JAVA_HOME, for shell builds
3. Toolchain JDK - default for source and target levels, as well as JDK used to run unit tests. I strongly recommend using this instead of sourceCompatibility, targetCompatibility and jvmTarget. 