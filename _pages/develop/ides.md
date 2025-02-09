---
mediawiki: IDEs
title: IDEs
section: Development:Tools:IDEs
---

{% include notice icon="info" content='

<center>

If you are new to programming, and wondering which IDE to try first:  
many [ImageJ](/software/imagej) and [Fiji](/software/fiji) developers use [Eclipse](/develop/eclipse) and the [command line](/develop/command-line).

</center>

' %} An {% include wikipedia title='Integrated development environment' text='integrated development environment'%} (IDE) is a software application that provides comprehensive facilities to computer programmers for software development. An IDE normally consists of a source code editor, build automation tools and a debugger. Most modern IDEs offer intelligent code completion features.

[ImageJ](/software/imagej) can be developed using any IDE which supports [Maven](/develop/maven), which includes:

<center>

{% include develop/ide-links %}

</center>

## Why use an IDE?

There are many advantages of using an IDE for software development:

1.  It is easy to access documentation about classes (i.e. [javadoc](/develop/source#javadocs)): just point your cursor over the name of the class, or press the keyboard shortcut (e.g., in Eclipse: {% include key keys='Shift|F2' %}).
2.  You can use code completion: just type the start of the class name, variable, method, etc you want to use and hit the keyboard shortcut (e.g., in Eclipse: {% include key keys='Ctrl|Space' %}).
3.  Compile errors are listed in a concise list; double-clicking on a line in that list will move the cursor to the problem.
4.  You can [debug your program interactively](/develop/debugging-exercises): just open the main class (i.e. a class having a `public static void main(String[] args)` method) and launch it in debug mode. E.g., in Eclipse: go to {% include bc path='Run|Debug As|Java Application'%}). This will switch to a different window layout (the *Debug perspective*) which offers you a range of views that are useful for debugging such as: local variables, thread stack traces, etc. You can interrupt the program at any time by clicking on the *pause* symbol and inspect the current state, single-step through the code and even to a limited amount replace code on-the-fly.
5.  The most important version control systems can be accessed easily through the IDE's GUI.
6.  There are many awesome keyboard shortcuts, especially effective to quickly explore large projects. (see e.g. [keyboard shortcuts for Eclipse](/develop/eclipse#keyboard-shortcuts)).
7.  They can be enhanced with plugins. E.g., for Eclipse, the [Vrapper plugin](http://vrapper.sourceforge.net/) adds a vim-like input scheme.

The main disadvantage of modern IDEs is that they are quite large and require a lot of resources—RAM and screen size in particular.

 
