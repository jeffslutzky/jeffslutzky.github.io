---
layout: post
title: "Back to the Future With Cron Jobs and Ruby Gems"
date: 2015-10-21 08:25:00 -0400
comments: true
categories: "Flatiron&nbsp;School Ruby&nbsp;gems cron&nbsp;jobs shell&nbsp;scripts"
---
It's <em>Back to the Future</em> Day!

<iframe width="840" height="473" src="https://www.youtube.com/embed/nT9Drje0Fwc" frameborder="0" allowfullscreen></iframe>

<em>Back to the Future</em> is my all-time favorite movie --  and the sequels are a close second and third -- so this is a pretty special day. To celebrate, I'll discuss time travel!

Sort of. I'll discuss cron jobs.

A cron job is a way to schedule tasks ahead of time in Unix-type command-line systems such as the Mac Terminal -- for example, sending out an email, updating a file at the same time every day, and so on. Tasks can be scheduled for a particular time or at regular intervals -- daily, weekly, etc.

I decided I wanted to post the following tweet on October 21, 2015, at 4:29 pm, to celebrate Marty McFly's arrival from 1985:

<pre>Yes, it's October 21, 2015 at 4:29 pm...

...on the East Coast.

But Hill Valley is in California. Three hours to go.

#BackToTheFutureDay</pre>

I wanted to preschedule my tweet, so it could go out even if I'm away from my computer at the time (as long as my computer is running). First I tried some web apps, but I couldn't get tweets to appear at the exact time I wanted. So I turned to the command line. Here's how this works.

<!-- more -->

<em>(1) Using the "t" Ruby gem</em>

The first step is to install <a href="https://github.com/sferik/t">a Ruby gem called t</a>, which provides command-line access to Twitter:

<pre>gem install t</pre>

After connecting your Twitter account to the gem by following the instructions on the gem's <a href="https://github.com/sferik/t">GitHub page</a>, you can tweet directly from the command line by typing 

<pre>t update "[text of your tweet]"</pre>

The text of your tweet will immediately show up on Twitter as a tweet from your account.

But what if you want to pre-schedule a tweet? What if you want to run the above command not now, but later? You'll need to set up a <a href="https://en.wikipedia.org/wiki/Cron">cron job</a>.

<em>(2) Setting up a cron job</em>

This involves two steps: (a) setting up a file that contains your "t" command, and (b) setting up the actual cron job.

<em>(a) Setting up the file containing your "t" command</em>

The best place to create this file (or shell script) is in your home directory. To find out where this is, type:

<pre>echo $HOME</pre>

Or you can go there directly by typing

<pre>cd $HOME</pre>

In your home directory, create a new file with a .sh extension, which is the extension for a shell script:

<pre>touch [filename].sh</pre>

Open this empty file in your text editor. At the top of the empty file, type

<pre>#!/bin/bash</pre>

(The exact path might be different on different computers, but this worked for me on a Mac.)

Next, you can type the "t" update command, but you have to preface the "t" with something. Ruby Version Manager (rvm) creates wrappers for gems, and you need to preface the "t" with the path for the wrappers. <a href="https://rvm.io/deployment/cron">This link</a> helped me figure that path out. For me, that path is /Users/jeffslutzky/.rvm/wrappers/ruby-2.2.3, so the full path to "t" is:

<pre>/Users/jeffslutzky/.rvm/wrappers/ruby-2.2.3/t</pre>

And therefore the full command is:

<pre>/Users/jeffslutzky/.rvm/wrappers/ruby-2.2.3/t update "[tweet text]"</pre>

So the full file is these two lines:

<pre>#!/bin/bash

/Users/jeffslutzky/.rvm/wrappers/ruby-2.2.3/t update "[tweet text]"</pre>

After creating the shell script, we have to change its permissions, by going back to the command line and typing

<pre>chmod +x [filename].sh</pre>

After creating the shell script and setting its permissions, the next step is to set up the cron job.

<em>(b) Setting up the cron job</em>

We're actually setting up a crontab, which is the text file that contains the schedule of tasks. I was having problems using my default text editor, Sublime, for crontabs, so I had to set my default crontab editor to Nano by typing this on the command line:

<pre>export VISUAL=nano; crontab -e</pre>

After that, open the crontab file by typing:

<pre>crontab -e</pre>

This file requires one line containing two things: a series of five numbers corresponding to the time we want the task to run, and the path to the shell script we just wrote.

The numbers come in this order: minutes, hours (in 24-hour format), days of month, months, days of week. I don't care about the day of the week, so I can replace that with an asterisk (*). For example, October 21 at 4:29 pm (i.e. 10/21 at 16:29) is:

<pre>29 16 21 10 *</pre>

That's just <em>10/21 - 16:29</em> in reverse order.

The path to my shell script is:

<pre>/Users/jeffslutzky/bttf.sh</pre>

So, adding this to the time, the full line to type in for this task is:

<pre>29 16 21 10 * /Users/jeffslutzky/bttf.sh</pre>

Save the file. That's it. The tweet is now pre-scheduled. We're all done!

To see the contents of the crontab, type:

<pre>crontab -l</pre>

To edit it again, type:

<pre>crontab -e</pre>

You can even add other tasks to the crontab file by adding a new line with a new time and shell script path.

I've actually pre-scheduled two tweets. If my calculations are correct, when this baby hits 88 miles per hour, you're gonna see some serious tweets (at <a href="http://twitter.com/tinmanic">twitter.com/tinmanic</a>).

To recap:

(1) Create a file with an .sh extension containing a command-line action, and set the file permissions.

<pre>#!/bin/bash

/Users/jeffslutzky/.rvm/wrappers/ruby-2.2.3/t update "[tweet text]"</pre>

(2) Create a new task in the crontab to execute the above file at a chosen time.

<pre>29 16 21 10 * /Users/jeffslutzky/bttf.sh</pre>

<em>Helpful links</em>

<ul>

<li><a href="https://github.com/sferik/t">the "t" Ruby gem</a></li>

<li><a href="https://developer.apple.com/library/mac/documentation/OpenSource/Conceptual/ShellScripting">Mac Developer Library: Shell Scripting Primer</a></li>

<li><a href="https://rvm.io/deployment/cron">Using RVM with Cron</a></li>

<li><a href="http://www.backtothefuture.com/">Back to the Future fan site</a></li>

<li><a href="http://www.october212015.com/">Countdown to Back to the Future Day</a></li>
</ul>

<img src="/images/delorean.jpg">