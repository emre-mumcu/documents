# How to Use Physical Device as Emulator

When you use your physical device as an emulator in Flutter, you run your app directly on the device instead of using a virtual emulator. 

Here's how you can do it:

## Install Google USB Drivers

Open Android Studio and install Google USB Driver in SDK Tools from SDK Manager window.

## Set up your device for development

1.  Enable Developer Options on your Android device: 
    Go to Settings > About phone > Tap Build number (OS Version) 7 times to enable Developer options.

2.  Enable USB Debugging:
    In Settings (or Additional Settings) > Developer options, turn on USB debugging.

3.  Open Developer Settings and allow USB Debugging and USB install settings.    

3.  Connect your device to your computer via USB.

## Connect your Device

Connect your Android Device to the PC and run the following command to verify the device is connected.

```shell
PS> flutter devices
```

**NOTE:** Before running the `flutter devices` command, I recommend disconnection all unnecessary USB devices (like flash drives, USB disks, card readers etc.)

You may need to allow access to the computer from the device screen.

## Run the Project

Run the flutter project selecting your physical deivce from the device list.
