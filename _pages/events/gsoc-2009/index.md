---
title: SoC 2009 Ideas
---

# Welcome!

Fiji is planning on applying to the Google Summer of Code 2009 program. As mentoring organizations have not yet been accepted, there is no guarantee that Fiji will be asked to participate. This page is to help plan proposed student projects. (UPDATE: Fiji [has been accepted](http://socghop.appspot.com/program/accepted_orgs/google/gsoc2009) as a mentoring organization. See the [Fiji GSoC organization page](http://socghop.appspot.com/org/show/google/gsoc2009/fiji).)

This page contains project ideas culled from the Fiji user and developer community. You can get started by reading some project descriptions, and the mailing list thread(s) that spawned them. Also consider joining the developer mailing list, or finding us on IRC. Details can be found in [Help](/discuss).

Don't like a project you see here? Just throw your ideas at us, on the developer mailing list!

# General Requirements

All projects have the following basic requirements:

-   Unless otherwise stated, projects will require programming in Java.
-   All materials must be released under the [GNU General Public License (GPL)](http://www.gnu.org/copyleft/gpl.html), version 2.
-   Individual students shall retain copyright on their works.
-   Projects must be tracked and managed in Git, and published on [1](https://fiji.sc/cgi-bin/gitweb.cgi) (we will provide you with an account).
-   Weekly project status reports should be sent to the project's mentors. Each status report should outline what was accomplished that week, any issues that prevented that week's goals from being completed, and your goals for the next week. This will help you to break your project down into manageable chunks, and will also help the project's mentors to better support your efforts.

Interested students are encouraged to read the [Advice for GSoC Students Page](http://code.google.com/p/google-summer-of-code/wiki/AdviceforStudents), as it has excellent suggestions that might help you to pick a project and shape your proposal.

If your proposal is accepted by the Fiji Development Community you will be expected to work on it full time during the summer. It is cool if you want to take a week off for vacation, but remember that Google is hiring you for the summer to help us improve Fiji. That should be your focus. Don't expect that you will be able to work on your project for just 10 hours a week and then collect at the end.

If your original proposal doesn't pan out or becomes too much of a challenge, you should work with your mentor to help redefine it. We really want to see every project succeed this summer, as there is a great deal of interest in these projects from within the user community.

Students can apply for the program at the [Google Summer of Code website](http://code.google.com/soc/). Please consider reviewing our [SoC2009Template](template) and answering its questions as part of your application.

# Project ideas

## General-purpose Image Processing Framework

ImageJ has an abstraction of an image, called ImageProcessor, which is specialized by data type as ByteProcessor, ShortProcessor, ColorProcessor... That only wraps 2-dimensional images, though. For 3-dimensional images, you need to use instances of the ImageStack class, and if you need even more dimensions, you need a HyperStack.

This is a bit complicated for something as simple as an image data container.

So some data structure is sorely needed, preferably with performant yet easy accessors to the data and metadata.

The idea is to have

-   an abstraction of the data type: a class that provides methods for performing arithmetics, to be specialized for all data types (byte, short, int, float, double, RGB, complex float and complex double),

<!-- -->

-   an abstraction of a "cursor": a class that points to a certain location in an image,

<!-- -->

-   a family of iterators that work on cursors,

<!-- -->

-   interpolation, cropping and affine/non-linear transformations implemented in terms of iterators,

<!-- -->

-   all kinds of cool stuff that will be now easy.

Careful implementation of these ideas should result in a framework where you do not need to implement the same algorithm for every single data type, where the code is still readable, and which executes as fast as if the algorithm was implemented once per data type.

This framework should also allow implementation of an efficient container for huge image data that keeps only a small fraction of the image in memory.

The minimal goal is a set of "data type", "cursor" and "iterator" implementations that wrap the available image types in ImageJ.

*Note*: this project idea is already pretty detailed (so there will be less need for the student to come up with own ideas, or with researching existing code), but the implementation will require some determination (the project is not trivial). To prove that the project will go anywhere, the student will have to add something in the project application in addition to what is written here, to demonstrate the capability of finishing the project.

**Goal:** Design and implement an efficient image framework.  
**Language:** Java.  
**Mentor:** Johannes Schindelin (johannes.schindelin@gmx.de)  

## Integrate Micro-Manager into Fiji

This project requires a bit of knowledge in compiling C++ code on Linux, MacOSX and Windows. The idea is to make a recipe that other people can use to compile new releases of [Micro-Manager](http://www.micro-manager.org/), as well as integrate it into the Fiji project for a smooth user experience. To ensure that support for Micro-Manager is not broken inadvertently, you shall add regression tests, too.

**Goal:** Provide an easy way to compile and ship Micro-Manager with Fiji.  
**Language:** Java  
**Mentor:** Johannes Schindelin (johannes.schindelin@gmx.de)

## Add more regression tests

Whenever releasing Fiji, or after major changes, we would like to be reasonably safe that we did not break things that used to work before. We already have a few [tests](https://fiji.sc/cgi-bin/gitweb.cgi?p=fiji.git;a=tree;f=tests), but hardly enough.

This project involves making a list of all the functionality Fiji provides already, and ideally to implement basic tests for all of them.

The idea is to extend the simple Python script in tests/record.py to make it easier to record what is expected to appear, and to provide faked user input.

**Goal:** Provide tests that can be run in headless mode, ensuring certain functionality does not get broken.  
**Language:** Java, Clojure, Javascript, Jython, JRuby **or** ImageJ's macro language  
**Mentor:** Johannes Schindelin (johannes.schindelin@gmx.de)

## Add word expansion to the scripting interpreters

Fiji offers scripting in 5 different languages, all running on the JVM: [Javascript](/scripting/javascript), [Jython](/scripting/jython), [JRuby](/scripting/jruby), [Clojure](/scripting/clojure), Beanshell and the ImageJ Macro Language. Through the reflection API and the numerous language hooks that each scripting engine provides, it is possible to complete method names or names of member variables, as well as class names.

**Goal:** Provide word-expansion capabilities to the scripting interpreters (see [Scripting Help](/scripting)).  
**Language:** A combination of Java plus the scripting language, in this order of preference: Javascript, Jython, JRuby, Clojure and Beanshell. If you can do them all, we'll erect you a monument.  
**Mentor:** [Albert Cardona](http://www.ini.uzh.ch/people/acardona) (acardona@ini.phys.ethz.ch)  

## Add a simple yet minimally powerful plugin and script editor

ImageJ offers currently a very limited, java.awt.TextArea -based built-in text editor, for writing, compiling and running java plugins, ImageJ macros, and as of recently javascript scripts. The text editor lacks syntax highlighting, does not use a proper font for coding, and lacks minimal debugging capabilities (such as parsing stack traces to highlight lines that failed to compile).

This project should be done in such a manner that it can be extended to allow debugging (single stepping, break points, stack traces).

The student will have to research if there are existing Open Source components that already implement a substantial amount of the functionality, to avoid reinventing the wheel.

**Goal:** Build a minimal text editor with a good font for coding, syntax highlighting for java and javascript (and optionally jruby, clojure and beanshell), and the ability to compile java code and parse stack traces to highlight lines that failed to compile. Building in [jvi](http://jvi.sourceforge.net) a big bonus, but must enable <i>non-modal</i> editing capabilities with implicit minimal emacs keybindings (like in any default MacOSX text area: {% include key keys='Ctrl|A' %}, {% include key keys='Ctrl|E' %}, {% include key keys='Ctrl|W' %}, etc.)  
**Language:** Java or a JVM scripting language of your choice (preferrably Javascript, Jython, JRuby or Clojure).  
**Mentor:** [Albert Cardona](http://www.ini.uzh.ch/people/acardona) (acardona@ini.phys.ethz.ch)  

## Enhance Fiji's plugin manager

Beyond the built-in commands, ImageJ provides the means to add user-developed plugins by implementing the PlugIn, PlugInFilter and PlugInFrame interfaces. Fiji uses ImageJ at the core and packages around it a very large number of plugins (see the [list of extensions](/list-of-extensions)). The ideal application menus cannot contain hundreds of plugins: any specific user has no use for more than half of them, and their mere presence get on the way to adding other, user-desired plugins.

The core idea of this project is to generate a plugins manager, inspired in the excellent plugin manager of [JEdit](http://www.jedit.org), to enable/disable plugins.

The lack of a plugin manager system is an enormous drag on the ability of good plugins to be widely distributed, and the ability of needy users to find them. The ability to be registered will also ensure proper compatibility with specific ImageJ versions, and the ability to track plugin versions.

At the moment, there is a rudimentary {% include github repo='fiji' branch='master' path='src-plugins/Fiji\_Updater/UpdateFiji.java' label='plugin updater' %} which should be a good starting point for this project. The list of current plugins used by that plugin can be found [here](http://update.fiji.sc/current.txt).

**Goal:** Convert Fiji's update plugin into a proper plugin manager.  
The GUI should have two main components: a tree with the plugins and a text pane for reading documentation.

-   The tree must mirror the user's plugins menu and provide the means to enable/disable plugins, i.e. to add/remove them from the menus. As well as indicate whether any updates are available for a plugin, or newly available plugins.
-   The text pane will offer the documentation on a plugin, and thus double as built-in plugin documentation GUI. The documentation must be fetched directly from this wiki site (displayed in a javax.swing.JEditorPane with an HTMLEditorKit).  

The plugin manager will query a specific server based on fiji.sc for updated and newly available plugins.

**Language:** Java or a JVM scripting language of your choice (preferrably Javascript, Jython, JRuby or Clojure).  
**Mentor:** [Albert Cardona](http://www.ini.uzh.ch/people/acardona) (acardona@ini.phys.ethz.ch), Johannes Schindelin (johannes.schindelin@gmx.de)  

## Provide cluster support for Fiji

Fiji runs fine on desktop machines, but for some tasks, it is better to use a cluster.

To that end, Fiji already supports "headless" mode, i.e. operation without the need to have a graphical user interface running. But that is not enough:

-   There must be an easy way to define what operation should be performed on what set of images, or on what set of subimages.
-   Clusters come in all kinds of flavors with a lot of different schedulers. A general backend with adapters for the most common schedulers will be needed.
-   The user should have a nice user interface to see the progress, and the end result.
-   For convenience, Fiji should offer the option to make sure that the current Fiji is installed.

**Goal:** Add a component to schedule processes.  
**Language:** Java.  
**Mentor:** Pavel Tomancak, Johannes Schindelin (johannes.schindelin@gmx.de)  

## Add JMathLib (MATLAB clone) support

Quite a few algorithms are available as proof-of-concept [MATLAB](/scripting/matlab) scripts. While it is [wrong to think of pixels as little squares](http://alvyray.com/Memos/CG/Microsoft/6_pixel.pdf), and literally all [MATLAB](/scripting/matlab) scripts to perform image processing are suffering from that shortcoming, it would be very nice nevertheless to be able to run the scripts without having to buy [MATLAB](/scripting/matlab) licenses just for that purpose.

Happily, there is a [MATLAB](/scripting/matlab) clone written in Java: [JMathLib](http://www.jmathlib.de/). While it is apparently not a speed demon, it should be useful to add JMathLib as a new scripting language to ImageJ, and integrate it into Fiji so that [MATLAB](/scripting/matlab) scripts can be executed just like all other ImageJ scripts, too.

The project would consist of

-   getting as many .m scripts for image processing as possible,

<!-- -->

-   integrating JMathLib as a script language into Fiji (using the infrastructure shared by Jython, JRuby, Clojure, Javascript and BeanShell) -- I suggest having a look at {% include github repo='fiji' branch='master' path='src-plugins/JRuby/JRuby\_Interpreter.java' label='the JRuby Interpreter' %} for an example,

<!-- -->

-   adapting (or overriding) JMathLib's image toolbox so that it integrates seamlessly with ImageJ,

<!-- -->

-   test (and fix what does not work) as many .m scripts as possible.

**Goal:** Integrate JMathLib as a new scripting language.  
**Language:** Java.  
**Mentor:** Johannes Schindelin (johannes.schindelin@gmx.de)  

# Other Resources

* [SoC2009Application](application)

# Other links

* [ImageJ's homepage](/)  
* [Fiji's developer mailing list](http://groups.google.com/group/fiji-devel)  
* [#fiji-devel channel](/discuss/chat#irc) on irc.freenode.net
