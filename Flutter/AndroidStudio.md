JetBrains Runtime (JBR)
The JetBrains Runtime (JBR)  is an enhanced JDK, distributed with Android Studio. It includes several optimizations for use in Studio and related JetBrains products, but can also be used to run other Java applications.

How do I choose a JDK to run Android Studio?
We recommend that you use the JBR to run Android Studio. It's deployed with and used to test Android Studio, and includes enhancements for optimal Android Studio usage. To ensure this, don't set the STUDIO_JDK environment variable.

The startup scripts for Android Studio look for a JVM in the following order:

1. STUDIO_JDK environment variable
2. studio.jdk directory (in the Android Studio distribution)
3. jbr directory (JetBrains Runtime), in the Android Studio distribution. Recommended.
4. JDK_HOME environment variable
5. JAVA_HOME environment variable
6. java executable in the PATH environment variable

How do I choose which JDK runs my Gradle builds?
If you run Gradle using the buttons in Android Studio, the JDK set in the Android Studio settings is used to run Gradle. If you run Gradle in a terminal, either inside or outside Android Studio, the JAVA_HOME environment variable (if set) determines which JDK runs the Gradle scripts. If JAVA_HOME is not set, it uses the java command on your PATH environment variable.

For the most consistent results, make sure you set your JAVA_HOME environment variable, and Gradle JDK configuration in Android Studio to that same JDK.

Gradle JDK configuration in Android Studio
To modify the existing project's Gradle JDK configuration, open the Gradle settings from File (or Android Studio on macOS) > Settings > Build, Execution, Deployment > Build Tools > Gradle.

Be aware that there are multiple JDKs in play:

1. JDK that runs Android Studio - let it default to the JetBrains Runtime (JBR) that comes with Android Studio

2. JDK that runs Gradle - set it in Android Studio for Studio builds or via org.gradle.java.home in gradle.properties, which defaults to JAVA_HOME, for shell builds

3. Toolchain JDK - default for source and target levels, as well as JDK used to run unit tests. I strongly recommend using this instead of sourceCompatibility, targetCompatibility and jvmTarget. 

