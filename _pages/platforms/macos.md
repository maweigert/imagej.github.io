---
title: MacOS
section: Learn:ImageJ Basics:Supported Platforms
---

{% capture macos-caption %}
{% include wikipedia title='Think different' text='Think different' %}.
{% endcapture %}
{% include img src='icons/macos' align='right' width=150 caption=macos-caption %}

{% include wikipedia title='macOS' text='macOS' %} (formerly called Mac OS X, then OS X) is {% include wikipedia title='Apple Inc.' text='Apple' %}'s desktop operating system. It is [the second most common desktop computing platform](https://www.netmarketshare.com/operating-system-market-share.aspx) after [Windows](/platforms/windows). This page details issues specific to using [ImageJ](/software/imagej) on macOS systems.




# Installation

See also the [Java 8](/news/2016-05-10-imagej-howto-java-8-java-6-java-3d) page for OS-X-specific issues.

# Troubleshooting

See also the [Troubleshooting](/learn/troubleshooting) page.

## ImageJ becomes very slow after running for a while

There are several reasons ImageJ can run slowly on macOS.

### Java painting bug

On macOS, older versions of Java 8 (prior to 1.8.0\_45)—as well as all versions of Java 7 (including 1.7.0\_80)—are extremely slow at displaying images. You should either upgrade to the latest version of Java 8, or revert to Java 6 (see "Frequently Asked Questions" below).

### Window menu bar bug

There is a bug in Java 8 on MacOS which causes the application to drastically slow down as many windows are opened and closed over time. Make sure you are using the latest version of Java 8, as well as the latest version of ImageJ.

### App Nap

With macOS version 10.9 "Mavericks" and later, there is an "App Nap" feature which dramatically slows down applications that are not in the foreground. Leave ImageJ in the foreground while it is processing to avoid this issue. (There are also [various](http://osxdaily.com/2014/05/13/disable-app-nap-mac-os-x/) [ways](http://www.cultofmac.com/274396/disable-app-nap-specific-apps-os-x-tips/) to disable App Nap on your machine, but we have not had much success with them. If you find a solution that works, allowing ImageJ to run fast in the background, please [tell us on the forum](http://forum.imagej.net/)!)

## No title bar in file chooser dialogs

On macOS 10.11 "El Capitan" and later, the operating system no longer includes a title bar for file chooser dialogs. See e.g. [this JDK bug](https://bugs.openjdk.java.net/browse/JDK-8136427) discussing the issue.

As a workaround, you can check "Use JFileChooser to open/save" in the {% include bc path='Edit | Options | Input/Output...'%} dialog.

# Frequently Asked Questions

See also the [Frequently Asked Questions](/learn/faq) page.

## How do I run ImageJ with Java 6?

It is unfortunately no longer feasible to install Apple Java 6 on current versions of macOS. However, ImageJ should work OK with Java 8. If you have difficulties, please post on the [Image.sc Forum](https://forum.image.sc/).

At any time, you can verify which Javas are installed on your system using {% include github org='ctrueden' repo='ctr-scripts' branch='master' path='java-info' label='this script' %}.

## How do I run ImageJ on Yosemite?

Install the [Java 8 JRE](http://java.com/) or [Java SE 8](http://www.oracle.com/technetwork/java/javase/downloads/).

## How do I run ImageJ on El Capitan?

Unfortunately, El Capitan has some new java-related issues. If you upgraded to El Capitan and your Java 8 installation is not being detected properly:

1.  Try installing the [Java SE JDK](http://www.oracle.com/technetwork/java/javase/downloads/index.html).
2.  If that does not work, see [this guide](https://oliverdowling.com.au/2015/10/09/oracles-jre-8-on-mac-os-x-el-capitan/) for steps which could get things working again.
3.  Alternately, ImageJ still works on El Capitan with Java 6 (see above).

# Additional macOS Tips

{% include warning/outdated %}

## Running Fiji in 32-bit mode

{% include notice icon="warning" content='It may no longer be possible to start Fiji in 32-bit on recent versions of macOS. See [this bug report](https://fiji.sc/bug/1018) for details.' %} Fiji on Intel Macs runs on Java 1.6 in 64-bit mode. If you need to run it in 32-bit mode, you can do so as follows:

1.  Launch Fiji normally, and choose {% include bc path='Edit | Options | Memory & Threads'%}.
2.  Reduce your Maximum Memory setting to \~1800 MB or less.
3.  Quit Fiji.
4.  {% include key keys='Ctrl|Left Click' %} the Fiji dock icon and choose {% include bc path='Options | Show in Finder'%}.
5.  {% include key keys='Ctrl|Left Click' %} on the Fiji application icon that pops up, and choose Show Info (or press {% include key keys='Cmd|I' %}).
6.  Check the "Open in 32-bit mode" box in the Fiji info window.
7.  Press the red X on the Fiji info window to close it.
8.  Launch Fiji again, and the status bar should report "\[32-bit\]" in brackets.

Alternately, you can execute the following code from the Terminal:

`arch -i386 /Applications/Fiji.app/Contents/MacOS/ImageJ-macosx`

Either way, you will need to make sure your maximum memory limit is set below \~1800 MB. If your maximum memory is set higher than the 32-bit limit, Fiji will not be able to start up successfully in 32-bit mode.

## Limited PowerPC (G4/G5) Mac support

We offer [a special intermediate release of Fiji specific to PowerPC Macs (G4/G5)](https://downloads.imagej.net/fiji/Heidelberg/fiji-macosx-ppc-20100802.dmg).

{% include notice icon="note" content="There is no Java 1.6 for PowerPC from Apple, meaning that Java comes at a considerable performance penalty on this platform. In addition, we will not be able to support Java versions prior to Java 1.6 at some stage, since that version offers a few features we want to rely on, such as [a versatile scripting framework](http://java.sun.com/developer/technicalArticles/J2SE/Desktop/scripting/)." %}

### Advanced

You can also install a third party Java 6, part of the OpenJDK project. You will need a [working X11 server](http://developer.apple.com/opensource/tools/x11.html), that you can find on your macOS disk, and MacPorts.

Execute **sudo port install openjdk6** on your Terminal. You can also install the SoyLatte Binaries, as an alternate choice. Then you can proceed with the generic Fiji Installation

Check more info at [landonf.bikemonkey.org/static/soylatte/](http://landonf.bikemonkey.org/static/soylatte/)

## Accessing the plugins and macros folders

To access the plugins or macros folders, set the Finder window to either icons or lists mode, <b>not</b> in column mode, and double-click them.

Alternatively, right-click (or {% include key keys='Ctrl|Left Click' %}) the Fiji.app and select "Show package contents", to open the folder where the actual plugins and macros folders are.

## Adding new plugins and macros

For plugins, please follow the instructions about [Installing 3rd party plugins](/plugins#installing-plugins). Otherwise, access the plugins folder as explained above and just drag and drop any plugin into the plugins folder, like you would do for ImageJ. Same for macros.

## Installing OpenJDK for macOS

As of JavaSE 7, Oracle now supports macOS [officially](http://www.h-online.com/open/news/item/Java-SE-7-Update-6-hands-OS-X-support-to-Oracle-1667714.html).

It is based on [an Apple-backed effort](http://openjdk.java.net/projects/macosx-port/) to get a proper macOS backend into the BSD port of OpenJDK. So far, only Snow Leopard and later are supported, and preliminary builds can be found [here](http://code.google.com/p/openjdk-osx-build/).

If you are experiencing problems, say, with AWT-AppKit related crashes, and if you do not mind working with an X11-based graphical user display, you might want to try OpenJDK.

As of mid-April 2011, OpenJDK for macOS has basic working support for Aqua, which you have to activate explicitly by passing the Java option *-Dswing.defaultlaf=com.apple.laf.AquaLookAndFeel*.

Since the development of OpenJDK for macOS is driven exclusively by Apple employees, the minimal macOS version required to run OpenJDK/Aqua is 10.6. If you require Fiji to run on earlier versions of macOS, you will have to go back to [SoyLatte](http://landonf.bikemonkey.org/static/soylatte/), where you will also find an X11-only OpenJDK version that runs on Mac OS X 10.5/PowerPC (macOS 10.6+ does not support PowerPC). In the alternative, you can put in a considerable effort to "backport" OpenJDK :-).

## Running Fiji in the command line

Often it is necessary to run Fiji in the command line, e.g. to pass some command-line options. To do so, start a Terminal (in the Finder, *Go&gt;Utilities*), and switch to the correct directory using the *cd* command. Note that the application itself is actually a directory called *Fiji.app*. For example, if you installed Fiji into */Applications* as recommended, do this:

`cd /Applications/Fiji.app`

If you unpacked Fiji onto your desktop, do this:

`cd $HOME/Desktop/Fiji.app`

Once you switched to the correct directory, start the Fiji launcher:

`Contents/MacOS/ImageJ-macosx`

{% include notice icon="note" content="A backslash (`\\`) is not the same as a slash (`/`). So: `Contents\\MacOS\\ImageJ-macosx` will **not** work." %}

Now you can pass, say, [Java Options](Java_Options):

`Contents/MacOS/ImageJ-macosx -verbose:gc --`

{% include notice icon="note" content="To distinguish between options intended for Java and options intended for ImageJ, you need to separate the former from the latter with a double-dash: `--`. Since the default is to accept ImageJ options, you have to pass a trailing double-dash if you want to pass only Java options." %}

## macOS keyboard shortcuts

It is often helpful to use keyboard shortcuts when using Fiji. There are also operating system specific shortcuts which can be quite helpful. For example, pressing {% include key keys='Command|Tab' %} and releasing first only the {% include key key='Tab' %} key will allow you to cycle through the running applications, while {% include key keys='Command|\`' %} will do the same for the windows opened by the current application. [Dave Polaschek](http://davespicks.com/) has [a comprehensive list](http://davespicks.com/writing/programming/mackeys.html).
