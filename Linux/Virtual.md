# Chrome

You can download the latest version of Google Chrome from their official website. For this, you can use the command-line with wget:

> wget https://dl.google.com/linux/direct/google-chrome-stable_current_amd64.deb

To install the package you've just downloaded, run the following command:

> sudo dpkg -i google-chrome-stable_current_amd64.deb

# yt-dlp

yt-dlp is a feature-rich command-line audio/video downloader with support for thousands of sites. The project is a fork of youtube-dl based on the now inactive youtube-dlc.

Install Using a Precompiled Binary

> sudo wget -O /usr/local/bin/yt-dlp https://github.com/yt-dlp/yt-dlp/releases/latest/download/yt-dlp
> sudo chmod a+rx /usr/local/bin/yt-dlp
> yt-dlp --version

FFmpeg is required for merging video and audio

> sudo apt install ffmpeg

Create a Download Script

> nano yv.sh

```
#!/bin/bash

if [ -z "$1" ]; then
    echo "Usage: $0 [YouTube URL]"
    exit 1
fi

YT_URL="$1"

# Run the yt-dlp command directly
yt-dlp -f "bestvideo[ext=mp4]+bestaudio[ext=m4a]/best[ext=mp4]" -o "%(title)s.%(ext)s" "$YT_URL"
```

Save and exit the editor by pressing CTRL+O, ENTER, and then CTRL+X. Then, make the script executable:

> chmod +x yv.sh

To use the script from any directory, move it to a directory in your PATH

> sudo mv yv.sh /usr/local/bin/yv

Then run the command as follows:

> yv https://...

ya.sh
```
#!/bin/bash

if [ -z "$1" ]; then
    echo "Usage: $0 [YouTube URL]"
    exit 1
fi

YT_URL="$1"

# Generate the yt-dlp command to download the best audio as MP3
# COMMAND="yt-dlp -f 'bestaudio' --extract-audio --audio-format mp3 -o '%(title)s.%(ext)s' \"$YT_URL\""

# Run the yt-dlp command directly
yt-dlp -f "bestaudio" --extract-audio --audio-format mp3 -o "%(title)s.%(ext)s" "$YT_URL"
```

yvfhd.sh
```
#!/bin/bash

if [ -z "$1" ]; then
    echo "Usage: $0 [YouTube URL]"
    exit 1
fi

YT_URL="$1"

# Run the yt-dlp command directly
yt-dlp -f "bestvideo[ext=mp4][height<=1080]+bestaudio[ext=m4a]/best[ext=mp4][height<=1080]" -o "%(title)s.%(ext)s" "$YT_URL"
```

yvhdr.sh
```
#!/bin/bash

if [ -z "$1" ]; then
    echo "Usage: $0 [YouTube URL]"
    exit 1
fi

YT_URL="$1"

# Run the yt-dlp command directly
yt-dlp -f "bestvideo[ext=mp4][height<=720]+bestaudio[ext=m4a]/best[ext=mp4][height<=720]" -o "%(title)s.%(ext)s" "$YT_URL"
```
