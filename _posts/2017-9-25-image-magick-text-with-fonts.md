---
layout: post
title: Create a text image with font styling using ImageMagick
---

On my mac I had to do a couple steps to get the Flipboard fonts to work. Search for the imagick_type_gen perl script. Follow the instructions from that to generate an XML file of your font directory. I had installed the Flipboard fonts (Fakt, Tiempos, etc.) into my personal Font Book. (I think when you double click a font file it will install it to your OS X Font book).

```
# this generates a type.xml file of stuff in my user dir.
find /Users/jlee/Library/Fonts/ -type f -name '*.*' | perl imagick_type_gen.pl -f - > ~/.magick/type-myfonts.xml


# now edit the imagemagick file. I think Mac does not have the option of reading from a ~/.magick directory so you must edit the original.
/usr/local/Cellar/imagemagick/7.0.4-6/etc/ImageMagick-7/type.xml


# I added this line to the XML file
<include file="/Users/jlee/.magick/type-myfonts.xml" />
```

Now imagemagick is configured! Here's a command line that worked for me.

```
convert -background none -fill "#e12828" \
  -font FaktFlipboardCon-Black -pointsize 72 \
  -density 150 label:"THE GOD WATCH" label-ffconblack.png
```

