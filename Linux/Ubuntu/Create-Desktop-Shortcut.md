# Manually create a .desktop shortcut in Ubuntu

In Ubuntu 24.04 and upcoming versions, desktop environments like GNOME handle desktop shortcuts differently than older versions.

You can manually create a .desktop file and put it on your Desktop.

The .desktop file format is standard for launching GUI apps.

The desktop icon feature must be enabled in GNOME. You can use gnome-tweaks to manage this:

```bash
sudo apt install gnome-tweaks
gnome-tweaks
```
Go to Desktop section and enable "Icons on Desktop".

Then, open a Terminal window and run the following:

```bash
vi ~/Desktop/firefox.desktop
```

Paste this content in to the file:

```text
[Desktop Entry]
Name=Firefox
Comment=Firefox Browser Dhortcut
Exec=firefox
Icon=firefox
Type=Application
Terminal=false
Categories=Network;WebBrowser;
```

Save and exit. 

Make it executable:

```bash
chmod +x ~/Desktop/firefox.desktop
```

Double-click it → Click "Trust and Launch" or "Allow Launching" if prompted.


# Installed .desktop files

Most apps already have a launcher in `/usr/share/applications/`.

To find Chromium’s launcher:

```bash
grep -i chromium /usr/share/applications/*.desktop
```

Then open the result:

```bash
vi /usr/share/applications/chromium-browser.desktop
```

Look for the line:

```text
Exec=chromium-browser %U
```

That’s what you’ll copy into your custom .desktop file.

You can also check what command runs the app:

```bash
which chromium-browser
```

If it's installed, you’ll get something like:

```bash
/usr/bin/chromium-browser
```

Then you can write:

```bash
Exec=chromium-browser
# Or use the full path if needed:
Exec=/usr/bin/chromium-browser
```

## How to Find the Icon

Option A: From existing .desktop file

cat /usr/share/applications/chromium-browser.desktop | grep Icon

You might see:

Icon=chromium-browser

Option B: Use full path to a custom icon

You can also use your own image file:

Icon=/home/yourusername/Pictures/chromium.png