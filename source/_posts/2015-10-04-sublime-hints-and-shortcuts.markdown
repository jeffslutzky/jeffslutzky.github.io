---
layout: post
title: "Sublime Hints and Shortcuts"
date: 2015-10-04 16:14:16 -0400
comments: true
categories: "Flatiron&nbsp;School"
---
I've completed my first week as a student in the <a href="http://www.flatironschool.com">Flatiron School's</a> Web Development Immersive program. It's been exciting and fun and busy and I've met friendly, interesting people. It's the best experience I've had in ages.

In this blog post I'll cover <a href="http://www.sublimetext.com/">Sublime</a>. Sublime is the default text editor we use at the Flatiron School; we use it to write all our Ruby programs, and I'm using it to write this very blog post. Besides Terminal, it's probably the program we use most often. Therefore, knowing some Sublime tips and shortcuts can help a programmer become much more efficient. One of our instructors sent us a link to a <a href="http://sublime-text-unofficial-documentation.readthedocs.org/en/latest/reference/keyboard_shortcuts_osx.html">whole bunch of Sublime shortcuts</a>. I thought I'd cover some of those, and some other tips I've found most useful so far:

<!-- more -->

<em>Setting up your visual environment.</em> Since you'll be staring at the Sublime interface all the time, you'll want the screen to look pleasant. The default environment for Sublime is light text on a black background. I find that to be hard on my eyes, but fortunately you can change this. In the menu, go to Sublime Text > Preferences > Color Scheme. Under this menu, Flatiron's Sublime installation has an option called Colorsublime-Themes, where you can pick Solarized Dark or Solarized Light. I use Solarized Light, which gives you a soothing beige background:

<img src="/images/solarized_light.png">

There are a ton of other options in the menu under Color Scheme - Default.

<em>Multiple column view.</em> Most Flatiron labs require getting certain tests to pass. It can be helpful to be able to look at your code and the test at the same time, especally when the test includes some data, such as a sample hash that you need to transform into another hash, or some supplied variables. To do this, you can press OPTION+CONTROL plus the number of columns you want. Pressing OPTION+CTRL+2 will give you two columns, and then you can drag any of the open tabs to any column:

<img src="/images/two_column_view.png">

Since this will give you narrower columns, you can then go to the menu bar and select View > Word Wrap so everything remains on the screen.

To switch back to single-column view, press OPTION+CTRL+1.

<em>Comment out a line from anywhere on that line.</em> Often I'm using binding.pry to look into my program, but then I want to stop using binding.pry and just see if my tests pass. Instead of repeatedly typing and deleting "binding.pry," you can just comment it out by typing COMMAND and the slash key (/). The nice thing is that you don't even have to be on the beginning of the line. If you type COMMAND + / from anywhere on that line, it will put a pound sign at the beginning of the line and comment out the whole line. To un-comment a line, just press COMMAND + / again.

<em>Replace every instance of some text with different text.</em> Sometimes you've named a variable but later decide you want to change it to something else. You can easily change every instance of the variable in your program. Just highlight any instance of the text, and <em>every</em> instance of that text will then have an outline around it:

<img src="/images/highlighted_text.png">

Now, press CTRL+COMMAND+G and you can then edit every single instance of that text at once! Everything you type will affect every instance of that text in your program:

<img src="/images/edited_text.png">

When you're done, click anywhere in your program to stop. So cool.

(Just be careful - if your program contains, for example, "item" and "items," you won't be able to edit "item" without also editing "items.")

<em>Move an entire chunk of text up or down.</em> If you want to move some text to a different part of your program - for example, if you want to rearrange the order of your methods - just highlight the lines of text you want to move, then click CTRL-COMMAND and the up-arrow or down-arrow. The entire chunk of text will move up or down a line each time you click.

There are so many other shortcuts in Sublime. It would be hard to <a href="http://sublime-text-unofficial-documentation.readthedocs.org/en/latest/reference/keyboard_shortcuts_osx.html">memorize them all at once</a>, but the more I use Sublime, the more I'll probably become familiar with them. You will, too.

Sublime is... sublime.
