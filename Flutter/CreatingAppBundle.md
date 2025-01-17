# Creating AppBundle

To publish on the Play Store, you need to sign your app with a digital certificate.

Android uses two signing keys: upload and app signing.

1.  Developers upload an .aab or .apk file signed with an upload key to the Play Store.

    Android requires that all APKs be digitally signed with a certificate before they are installed on a device or updated. When releasing using Android App Bundles, you need to sign your app bundle with an upload key before uploading it to the Play Console

2.  The end-users download the .apk file signed with an app signing key.

    With Play App Signing, Google manages and protects your app's signing key for you and uses it to sign optimized distribution APKs that are generated from your app bundles. Play App Signing stores your app signing key on Google’s secure infrastructure and offers upgrade options to increase security.

## Create a Keystore

Use the following command to create a keystore for signing:

```text
PS C:\Java\jdk-23\bin> .\keytool -genkey -v -keystore "C:\AppStore\Dev.Flutter\play_store\keystore\mumcusoftware.jks" -keyalg RSA -keysize 2048 -validity 10000 -alias mumcusoftware
```

This command creates a new keystore with the provided parameters provided by the user when prompted by the tool. 

By default, keystore and keyalias passwords are the same.

You can check and validate the keystore by running the following command:

```text
PS C:\Java\jdk-23\bin> .\keytool -list -v -keystore "C:\AppStore\Dev.Flutter\play_store\keystore\mumcusoftware.jks"
```

## Creating key.properties

The key.properties file in a Flutter/Android project is a configuration file used to securely store the details of the keystore (used for signing your app) and its credentials. This file helps you manage sensitive information like the keystore file path, passwords, and alias in a structured way, separating them from the main build files.

You need to create a properties file in order to keep the keystore and alias parameters safe. This is optional but highly recommended.

Don't forget to add the key.properties file in the .gitignore file to prevent accidentally sending the file to source cıntrol.

Create a new text file named "key.properties" in the android folder (where there is also a "local.properties" file exixsts) and add the following content in it:

```text
storeFile=C:/AppStore/Dev.Flutter/play_store/keystore/mumcusoftware.jks
storePassword=your_password
keyAlias=mumcusoftware
keyPassword=your_password
```

storePassword is the password for the entire keystore file. It is set when you create the keystore using the keytool command by prompting Enter keystore password prompt. keyPassword is the password for the specific key (alias) inside the keystore. Each key in the keystore can have its own password.

The keystore (.jks file) is like a container that holds one or more keys. The storePassword secures the keystore container itself. The keyPassword secures an individual key (identified by its alias) inside the keystore.

## Modift the build.gradle File

Open the build.gradle file located in the "android/app" folder and edit it as follows:

```text
plugins {
    // ...
}

def keystoreProperties = new Properties()
def keystorePropertiesFile = rootProject.file('key.properties')
// if (keystorePropertiesFile.exists()) { }
keystoreProperties.load(new FileInputStream(keystorePropertiesFile))

android {
    // ...

    signingConfigs {
        release {
            // To use key.properties
            storeFile keystoreProperties['storeFile'] ? file(keystoreProperties['storeFile']) : null
            storePassword keystoreProperties['storePassword']
            keyAlias keystoreProperties['keyAlias']
            keyPassword keystoreProperties['keyPassword']
            
            // To set parameters manually
            // storeFile file('C:/AppStore/Dev.Flutter/play_store/keystore/mumcusoftware.jks')
            // storePassword 'your_password'
            // keyAlias 'mumcusoftware'
            // keyPassword 'your_password'
        }
    }

    buildTypes {
        release {
            signingConfig = signingConfigs.release
            minifyEnabled true
            shrinkResources true
            proguardFiles getDefaultProguardFile('proguard-android-optimize.txt'), 'proguard-rules.pro'
        }
    }
}

flutter {
    source = "../.."
}
```

## Create the App Bundle

Run one of the following commands to create apk or bundle file:

```text
# Build APK:
flutter build apk --release

# Build AAB (Recommended for Play Store): 
flutter build appbundle --release
```

This command creates the bundle file "build\app\outputs\bundle\release\app-release.aab".

# References:

https://docs.flutter.dev/deployment/android