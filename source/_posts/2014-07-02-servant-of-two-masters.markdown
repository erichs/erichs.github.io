---
layout: post
title: "Servant of Two Masters"
date: 2014-07-02 11:08:36 -0400
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




[LastTime]: /blog/2014/06/30/broken-windows-and-broken-code/ "Broken Windows and Broken Code"
[manfc]: http://pubs.opengroup.org/onlinepubs/9699919799/utilities/fc.html
