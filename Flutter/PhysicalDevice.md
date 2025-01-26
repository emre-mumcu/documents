# Physical Device as Emulator

To use your physical device as an emulator in Flutter, you'll need to run your app directly on the device instead of using a virtual emulator. Here's how you can do it:

## 1. Install Google USB Drivers

Open Android Studio and install Google USB Driver in SDK Tools from SDK Manager window.

## 2. Set up your device for development

1.  Enable Developer Options on your Android device: 
    Go to Settings > About phone > Tap Build number (OS Version) 7 times to enable Developer options.

2.  Enable USB Debugging:
    In Settings (or Additional Settings) > Developer options, turn on USB debugging.

3.  Connect your device to your computer via USB.

## 3. Connect your Device

Connect your Android Device to the PC and run the following command to verify the device is connected.

```
flutter devices
```

## 4. Run the Project

Run the flutter project selecting your physical deivce from the device list.
