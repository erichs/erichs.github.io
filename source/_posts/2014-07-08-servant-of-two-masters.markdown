---
layout: post
title: "Servant of Two Masters"
date: 2014-07-08 14:08:36 -0400
published: true
comments: true
categories: Shell
---

[Last time][LastTime], I brought up the subject of fixing shell commands as you go along, and suggested that the practice of doing so could be good for your programmatic health. Today I'd like to highlight an underused Unix command that does this well, and talk through the broader implications of its use.

<!-- more -->

## The `fc` utility
First introduced via the Korn Shell, and a [standard Unix utility][manfc] since at least the mid-90's, `fc` is short for "fix command". Its purpose is to open up a previous command in the editor of your choice, and re-run the revised command once you are finished editing it.

With no arguments, `fc` will open the most previous command in the editor for editing and re-evaluation. In pure shell terms, it is roughly equivalent to:

``` bash I hope you'll agree, 'fc' is easier to type
echo !! > /tmp/cmd && $EDITOR /tmp/cmd && eval $(cat /tmp/cmd); rm /tmp/cmd
```

Most implementations of `fc` will use an editor specified in the `FCEDIT` environment variable, followed by `EDITOR`, then `vi`, and finally `ed`. You could of course launch an external editor like Sublime Text or Eclipse, but I find that keeping the editor inside the terminal feels most natural for this purpose.

I want to talk now about the experience of using a command like this. If you've not used this utility before, go ahead and try it out a few times. Yes, right now. Seriously, I'll wait.

<hr />
<center>{% youtube 9hmDZz5pDOQ %}</center>
<hr />

## Context Switching
So, kinda cool, but a little jarring, right? There you were happily plunking commands down at a prompt, and all of a sudden you're in your familiar editor, but perhaps in a slightly new context. Now you are editing your last command, say `ls -l`, as if it were a script:

``` bash
#!/bin/sh

ls -l
```

Furthermore, the act of saving and quitting ran the script, back at your terminal.

## Two Environments, Two Adaptations
I think the feeling of slight disorientation that occurs when using `fc` for the first time stems from inherent tension between the two modes of operation, or execution environments of the shell interpreter: the shell is at once the familiar REPL prompt we use all the time, and the shell is also the interpreter of script files.

### The Prompt
We naturally adapt our shell behaviors, as users, to these different execution environments: the interactive, REPL-nature of the prompt means that we can run a quick command, and immediately see the output displayed to the terminal. Based on that output, we might decide to try a different command altogether, consult a man page, or perhaps just hit the up arrow and make a quick revision to our command.

In particular, using the shell prompt to build up complex command pipelines is invaluable: you get to see the output of each filter command:

<script type="text/javascript" src="https://asciinema.org/a/10680.js" id="asciicast-10680" async data-speed="2"></script>

At the prompt, this feels natural. You try a command, examine the output or result, add an appropriate filter command or commands until you have massaged the output into a desired format. You have the benefit of **understandability** as a **writer** of the command because the output of all the intermediate commands are available to you **while** you are writing.

### The Script
Contrast that experience with encountering something like this in a script:

```bash Readability matters
#!/usr/bin/env bash
 netstat -p tcp | grep -i established | awk '{print $5}' | awk -F'.' '{ $NF="" } 1' | sed 's/ /./g' | sed 's/\.$//' | sort | uniq
```

The above is simply unintelligible. Even if you are a regex master and know the syntax for each of the above commands, you do not have the benefit of having written that command to begin with. The intent of all those pipeline filters is not communicated to the reader, and unless you actually played with that command, one step at a time, at the prompt, it is unlikely you would ever know what that command does.

**Pipes, as a design feature, are central the [Unix way][UnixWay]**. They are Unix's killer app. Unfortunately, they **mask the intent** of a command sequence. They are extremely convenient to use at a prompt, but end up being *write-only*.

The maintainer of a script with the above command sequence in it would be well-advised to break that command into sections, or at least encapsulate the pipeline in a descriptive function, like:

```bash Names give the reader a clue
#!/usr/bin/env bash

function unique_remote_connected_hosts {
  netstat -p tcp | grep -i established | awk '{print $5}' | awk -F'.' '{ $NF="" } 1' | sed 's/ /./g' | sed 's/\.$//' | sort | uniq
}

unique_remote_connected_hosts
```

or perhaps:

```bash Composition helps
#!/usr/bin/env bash

function unique_remote_connected_hosts {
  remote_connections | strip_port_number | sort | uniq
}

function remote_connections {
  netstat -p tcp | grep -i established | awk '{print $5}'
}

function strip_port_number {
  awk -F'.' '{ $NF="" } 1' | sed 's/ /./g' | sed 's/\.$//'
}

unique_remote_connected_hosts
```

This functional composition is easy to do in a text editor, the natural habitat of a script. It is awkward and difficult to do at the prompt, which is too bad.

More on resolving this fundamental tension, next time.

[LastTime]: /blog/2014/06/30/broken-windows-and-broken-code/ "Broken Windows and Broken Code"
[manfc]: http://pubs.opengroup.org/onlinepubs/9699919799/utilities/fc.html
[UnixWay]: http://en.wikipedia.org/wiki/Unix_philosophy
