---
layout: post
title: Create a text image with font styling using ImageMagick
---

These directions worked for me on Mac OS X (Sierra 10.12+). On my mac I had to do a couple steps to get the Flipboard fonts to work. Notes: I had installed imagemagick using Brew (`brew install imagemagick`)

You need to get imagemagick to recognize your fonts
=====

First, if you have some custom in-house created fonts, you need to add them to your Font Book. I had installed the Flipboard fonts (Fakt, Tiempos, etc.) into my personal Font Book. (I think when you double click a font file it will install it to your OS X Font book). If that doesn't work I think you can open Font Book and drag the font files (`.ttf` `.otf` `.woff` etc) onto the Font Book. Adding to Font Book will put your files into `~/Library/Fonts`.

Next, search the web for `imagick_type_gen` perl script and download it. The instructions will tell you to run the script to generate an XML dump of your font directory. Here's the commands I ran to make it work.

```
# this generates a type.xml file of stuff in my user font dir.
find ~/Library/Fonts/ -type f -name '*.*' | perl imagick_type_gen.pl -f - > ~/.magick/type-myfonts.xml


# now edit the system ImageMagick type.xml file. In the docs I read that Mac does not have the option of reading from a ~/.magick directory. I had to edit the original file that was configured by Brew.
/usr/local/Cellar/imagemagick/7.0.4-6/etc/ImageMagick-7/type.xml


# I added this line to the XML file
<include file="/Users/jlee/.magick/type-myfonts.xml" />
```

Now imagemagick is configured! Here's a command line that worked for me to produce an image. `convert` is an imagemagick command-line tool that should have been installed to your system with the brew install command.

```
convert -background none -fill "#e12828" \
  -font FaktFlipboardCon-Black -pointsize 72 \
  -density 150 label:"IMAGEMAGIC FONT TEXT" label-ffconblack.png
```

