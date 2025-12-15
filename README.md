# Using xfce4-appfinder to pick images.

I'll be using emojis/emotes for the examples and scripts as I have it set up that way.
You'll have to change the script to work on your machine.

Here how it looks when I run app finder and seach for the images ( emotes in this case ).
![](https://github.com/neekotism/xfce4-image-picker/blob/main/emote-picker.png?raw=true)

It's a very easy and convenient way to pick images and such without having to configure much.

The way is work is simple. You create desktop ( application ) entries for each emoji in **/home/user/.local/share/applications/** and it run an xclip script that copies the image to your clipboard when you open the desktop entry.

This is the config I have for the desktop entries.
```
[Desktop Entry]
Name=Emote {name}
Icon=/home/user/Emojis/{name}.png
Exec=/home/user/Scripts/emoji.sh {name}
Type=Application
Comment=
Path=
Terminal=false
StartupNotify=false
```

I have the xclip script set up in a seperate file instead of putting the xclip script in the entry itself so that I can update the script anytime I want without having to change the desktop entries.

This is the xclip script that copies the image to the clipboard.
```
#!/bin/bash
cd ~/Emojis
xclip -selection clipboard -t image/png -i "$1.png"
```

Of course! It's so much work to make an entry for each images/emojis so I made a script to automatically create desktop entries for all the png files that are inside of the /home/user/Emojis folder.

```Bash
#!/bin/bash

EMOJI_DIR="/home/user/Emojis"
SCRIPT_DIR="/home/user/Scripts"
DESKTOP_DIR="$HOME/.local/share/applications"

IMAGES=($(ls "$EMOJI_DIR"/*.png))

for IMAGE in "${IMAGES[@]}"; do
    NAME="${IMAGE##*/}"
    NAME="${NAME%.*}"
    DESKTOP_FILE="$DESKTOP_DIR/emote_$NAME.desktop"

    echo "[Desktop Entry]" > "$DESKTOP_FILE"
    echo "Name=Emote $NAME" >> "$DESKTOP_FILE"
    echo "Icon=$EMOJI_DIR/$NAME.png" >> "$DESKTOP_FILE"
    echo "Exec=$SCRIPT_DIR/emoji.sh $NAME" >> "$DESKTOP_FILE"
    echo "Type=Application" >> "$DESKTOP_FILE"
    echo "Comment=" >> "$DESKTOP_FILE"
    echo "Path=" >> "$DESKTOP_FILE"
    echo "Terminal=false" >> "$DESKTOP_FILE"
    echo "StartupNotify=false" >> "$DESKTOP_FILE"
done

echo "Desktop entries created in $DESKTOP_DIR"
```

