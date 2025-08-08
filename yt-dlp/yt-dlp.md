# yt-dlp

yt-dlp is a feature-rich command-line audio/video downloader with support for thousands of sites. The project is a fork of youtube-dl based on the now inactive youtube-dlc.

## Installation

### Option 1: Install via Python's pip (Recommended)

(This method ensures you get the latest version.)

```bash
$ sudo apt update
$ sudo apt install python3 python3-pip
$ pip install --upgrade yt-dlp
$ yt-dlp --version
```

### Option 2: Install Using a Precompiled Binary

(This method avoids dependency issues.)

```bash
$ sudo wget -O /usr/local/bin/yt-dlp https://github.com/yt-dlp/yt-dlp/releases/latest/download/yt-dlp
$ sudo chmod a+rx /usr/local/bin/yt-dlp
$ yt-dlp --version
```

Redownload the binary from the release link above to update the application.

### Option 3: Install from a Package Manager (Snap/Flatpak)

You can use Snap or Flatpak if you have them installed, but these versions may not be as up-to-date as the pip method.

```bash
$ sudo snap install yt-dlp
```

### FFMpeg

FFmpeg is required for merging video and audio:

```bash
$ sudo apt install ffmpeg
```

## How to Use

To download a video as an MP4 file with the best resolution and audio using yt-dlp, use the following command:

```bash
# To download videos in MP4 format in best video and best audio quality
$ yt-dlp -f "bestvideo[ext=mp4]+bestaudio[ext=m4a]/best[ext=mp4]" -o "%(title)s.%(ext)s" [video_url]

# To download videos in MP4 format and 1080p resolution
$ yt-dlp -f "bestvideo[ext=mp4][height<=1080]+bestaudio[ext=m4a]/best[ext=mp4][height<=1080]" -o "%(title)s.%(ext)s" [video_url]
```

**Explanation of the Command**

* -f: Specifies the format.
	* bestvideo[ext=mp4]+bestaudio[ext=m4a]: Downloads the best video in MP4 format and the best audio in M4A format, then merges them.
	* /best[ext=mp4]: Falls back to downloading the best single MP4 file if merging isn’t needed.
* -o "%(title)s.%(ext)s": Sets the output file name to the video title with the appropriate extension.
* [video_url]: Replace this with the URL of the video you want to download.

## Create a Download Script

To create a script that takes a YouTube link and returns the appropriate yt-dlp command, you can follow these steps:

```bash
$ vi yt-dlp-best.sh
```

```text
#!/bin/bash

# Check if a URL is provided
if [ -z "$1" ]; then
    echo "Usage: $0 [YouTube URL]"
    exit 1
fi

# Assign the provided URL to a variable
YT_URL="$1"

# Generate the yt-dlp command
COMMAND="yt-dlp -f 'bestvideo[ext=mp4]+bestaudio[ext=m4a]/best[ext=mp4]' -o '%(title)s.%(ext)s' \"$YT_URL\""

# Output the command
echo "Generated yt-dlp command:"
echo "$COMMAND"

# Optional: Run the command directly (uncomment the line below if desired)
# eval "$COMMAND"
# -EOF-
```

Save and exit the editor and make the script executable:

> $ chmod +x yt-dlp-best.sh

Provide a YouTube URL as an argument to the script:

> $ ./yt-dlp-best.sh https://www.youtube.com/watch?v=********

### Add to PATH (optional): 

To use the script from any directory, move it to a directory in your PATH (e.g., /usr/local/bin):

> $ sudo mv yt-dlp-best.sh /usr/local/bin/yt-dlp-best.sh

### Advanced Features

If you want the script to directly run the command instead of just outputting it, modify the script to execute yt-dlp:

```text
#!/bin/bash

if [ -z "$1" ]; then
    echo "Usage: $0 [YouTube URL]"
    exit 1
fi

YT_URL="$1"

# Run the yt-dlp command directly
yt-dlp -f "bestvideo[ext=mp4]+bestaudio[ext=m4a]/best[ext=mp4]" -o "%(title)s.%(ext)s" "$YT_URL"
```

### For the Best Audio as mp3 Only

```text
#!/bin/bash

if [ -z "$1" ]; then
    echo "Usage: $0 [YouTube URL]"
    exit 1
fi

YT_URL="$1"

# Run the yt-dlp command directly
yt-dlp -f "bestaudio" --extract-audio --audio-format mp3 -o "%(title)s.%(ext)s" "$YT_URL"
```

# MacOS

```zsh
% brew install ffmpeg
% brew install yt-dlp
```

**Download shell scripts**

* [Mp3](/Files/yt-dlp/yt-mp3)
* [Mp4 (720)](/Files/yt-dlp/yt-720)
* [Mp4 (1080)](/Files/yt-dlp/yt-1080)
* [Mp4 (Best)](/Files/yt-dlp/yt-best)

After downloading the scripts, allow them to be run: 

> chmod +x ~/Scripts/scriptadi.sh

### Add to PATH

Copy the downloaded scripts to a folder `$HOME/Scripts`

Check which shell you are using:

> echo $SHELL 

```bash
# zsh
% vi ~/.zshrc 
# bash
% vi ~/.bash_profile
```

Add the following PATH entry:

> export PATH="$HOME/Scripts:$PATH"

Apply the change:

```bash
# zsh
% source ~/.zshrc 
# bash
% source ~/.bash_profile
```

# References

* https://github.com/yt-dlp/yt-dlp